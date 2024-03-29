---
title: "Efficient Aggregate Pipeline Testing in MongoDB"
datePublished: Sat Dec 17 2022 06:57:41 GMT+0000 (Coordinated Universal Time)
cuid: clbrl7iqu000l08mk8c7t3o4j
slug: efficient-aggregate-pipeline-testing-in-mongodb
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1671260190195/b0UQB1IjU.jpeg
tags: mongodb

---

MongoDB provides a nifty way to test your aggregation pipeline before writing a single line of code from their UI. I use MongoDB Compass for this but you can also try this from the MongoDB Atlas UI.

Go to the collection you want to try it on &gt; Click on `Aggregation` &gt; Now add the stages you want to try

Let's take an example of getting Pinterest boards of users who have successfully onboarded using different stages:

1.  Stage 1: Get boards that are active (not deleted) using `$match`
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671259035650/g3uW7DpFF.png align="center")

2.  Stage 2: Use `$lookup` to look those users up from `users` collection and add them as `author`
    
3.  Stage 3: Use [$unwind](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/) to deconstruct the `author` array
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671259232481/9P9HtMV4n.png align="center")

*As you can see, with each stage we can see the resulting output. This helps a lot to tweak your query according to your need.*

4.  Finally, use the `$match` query to add a check for `onboardingDone` field
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671260372296/skBxIZWR9.png align="center")

After you are done with your query, you can easily export the pipeline you created using the export option

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671259682133/KKZMPWcWJ.png align="center")

Here's what the final pipeline looks like:

```typescript
[
  {
    '$match': {
      'isActive': true
    }
  }, {
    '$lookup': {
      'from': 'users', 
      'localField': 'user', 
      'foreignField': '_id', 
      'as': 'author'
    }
  }, {
    '$unwind': {
      'path': '$author'
    }
  }, {
    '$match': {
      'author.onboardingDone': true
    }
  }
]
```

* * *

As always, feel free to add suggestions or feedback :)