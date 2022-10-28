# React Query: Execution order of useMutation side effects

Today I learned, what is the actual order of execution of React Query `useMutation` side effects.

Given a mutation like this:

```js
  const mutation = useMutation(
    (id) => {
      console.log('Mutation callback')
      return toggleTodo(id)
    },
    {
      onMutate: () => {
        console.log('onMutate')
      },
      onSuccess: () => {
        console.log('onSuccess')
        queryClient.invalidateQueries('todos')
      },
      onSettled: () => {
        console.log('onSettled')
      },
    },
  )
```

I was expecting the order of execution to be:

```
// ❌ Wrong
Mutation callback
onMutate
onSuccess
onSettled
```

But it's actually:

```
// ✅ Correct
onMutate
Mutation callback
onSuccess
onSettled
```

This behavior is described in the [documentation](https://react-query-v3.tanstack.com/reference/useMutation#_top):
> `onMutate` - This function will fire before the mutation function is fired and is passed the same variables the mutation function would receive


## References 
* [React Query Docs - Mutations](https://react-query-v3.tanstack.com/guides/mutations)
* [CodeSandbox with example above](https://codesandbox.io/s/react-query-order-of-execution-usemutation-side-effects-r4odrv)