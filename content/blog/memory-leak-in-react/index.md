---
title: Solution for memory leak in React JS
date: "2020-12-15T22:12:03.284Z"
description: "This blog will provide you with the solution for the memory leak issue in react as well as I'll share my experience while facing the issue."
---

Hello everyone!!

Today we are going to resolve an issue that we faces many times while developing react apps using a backend api ,firebase etc.

When you update a state from an unmounted component, React will throw this error:

>Can't perform a React state update on an unmounted component. This is a no-op, but it indicates a memory leak in your application. To fix, cancel all subscriptions and asynchronous tasks in the componentWillUnmount method.


We are going to use useEffect hook in an elegant way so that this issue will be resolved.

 ### Solution-1:

```js
useEffect(() =>; {
    let unsubscribe = false;
    const runFunc = async () =&gt; {
      try {
        if (!unsubscribe) {
          // do the job
        }
      } catch (err) {
        if (!unsubscribe) {
          throw err;
        }
      }
    };

    runFunc();

    return () => {
      unsubscribe = true;
    };
  }, [...]);
```

 ### Solution-2:While using firebase

I was building an Instagram clone using firebase and hooks. Whenever I tried to make a request to the firebase firestore, I would always received an error message that I had a memory leak in my certain component. I used to ignore that at first because my app was still running fine. But one day, it stopped completely. So at the time I tried to find out the solution to this problem.After reading the official firebase docs ad other different answers from stackoveflow, I came to a point that I was updating a state of an unmounted component and wasn't cleaning up my susbscriptions.

Finally, I came upto an elegant solution for this:

```js
 useEffect(() => {
    //Subscribe is done here
    const cleanUp = firebase.firestore().collection('posts') .doc(id)
        .onSnapshot( doc=> {
            setPosts(doc)
        }, err => { setError(err); }
    );
    return () => cleanUp(); //Unsubscribe is done here
 }, []);
```

If you doesn't unsubscribe your firebase channel then you may receieve unexpected errors.


###Solution-03 : While using axios

The issue I faced while using firebase , the same issue I faced again while creating Netflix clone using TMDB api. But this time challenge was bit different because this time I was using axios. Since I havent faced this issue with axios before, it was a challenge for me. The first thing I did was to visit the official docs of axios and guess what I got the answer of the same issue there.

This time  I didn't cancelled the unwanted requests which led to this issue.So what I did was created a cancel token using Canceltoken.source() .

```js
  useEffect(() => {
  const source = axios.CancelToken.source();

  const fetchData = async () => {
    try {
      const response = await axios.get("/results", {
        cancelToken: source.token
      });
      // ...
    } catch (error) {
      if (axios.isCancel(error)) {
        //cancelled
      } else {
        throw error;
      }
    }
  };

  fetchData()

  return () => {
    source.cancel();
  };
}, [results]);
```


From my experience of facing this issue, I came upto a conclusion that -
>the useEffect function is somewhat similar to the componentDidMount lifecycle method in React class components.

And

>the cleanup function is similar to the componentWillUnmount lifecycle method of the class components.

I hope that you somewhat got an idea for the solution of this memory leak issue.

Thank you!! Have a great day!!


