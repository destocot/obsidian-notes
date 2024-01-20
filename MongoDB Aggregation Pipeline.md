
# Question 1: How many users are active?
  
```
[
  {
    $match: {
      isActive: true,
    },
  },
  {
    $count: "activeUsers",
  },
]
```

> create a variable named activeUsers of our own choosing.  
### Output

```
{
  "activeUsers": 516
}
```

---

# Question 2: What is the average age of all users?

```
[
  {
    $group: {
      _id: null,
      averageAge: {
        $avg: "$age"
      }
    }
  }
]
```

> _id: null, groups all our documents into a single document allowing us to perform an average of all our data

### Output

```
{
  "_id": null,
  "averageAge": 29.835
}
```

---

# Question 3: List the top 5 most common favorite fruits among users

```
[
  {
    $group: {
      _id: "$favoriteFruit",
      count: {
        $sum: 1
      }
    }
  },
  {
    $sort: {
      count: -1
    }
  },
  {
    $limit: 5
  }
]
```

> $sum: 1, adds 1 to the count for each grouping of the favoriteFruit fields.

> $sort: { count: -1 } sorts the data by descending order

> $sort: { count: 1 } sorts the data by ascending order
### Output

```
{
  "_id": "banana",
  "count": 339
}
```

---
# Question 4: Find the total number of males and females

```
[
  {
    $group: {
      _id: "$gender",
      genderCount: {
        $sum: 1
      }
    }
  },
]
```
### Output

```
{
  "_id": "female",
  "genderCount": 507
}
```

```
{
  "_id": "male",
  "genderCount": 493
}
```
---

# Question 5: Which country has the highest number of registered users?

```
[
  {
    $group: {
      _id: "$company.location.country",
      usersInCountryCount: {
        $sum : 1
      }
    }
  },
  {
    $sort: {
      usersInCountryCount: -1
    }
  },
  {
    $limit: 1
  }
]
```

### Output

```
{
  "_id": "Germany",
  "usersInCountryCount": 261
} 
```

---

# Question 6: List all the unique eye colors present in the collection.

```
[
  {
    $group: {
      _id: "$eyeColor"
    }
  }
]
```

### Output

```
{
  "_id": "brown"
}
```

```
{
  "_id": "green"
}
```

```
{
  "_id": "blue"
}
```

---

# Question 7: What is the average number of tags per user?

**method 1**

```
[
  {
    $unwind: "$tags"
  },
  {
    $group: {
      _id: "$_id",
      numberOfTags: {
        $sum: 1
			}
    }
  },
  {
    $group: {
      _id: null,
      averageNumberOfTags: {
	      $avg: "$numberOfTags"
      }
    }
  }
]
```

> unwind separates with array fields

### Output

```
{
  "_id": null,
  "averageNumberOfTags": 3.556
}
```

**method 2**

```
[
  {
    $addFields: {
      numberOfTags: {
        $size: {
          $ifNull: ["$tags", []]
        }
      }
    }
  }, 
  {
    $group: {
      _id: null,
      averageNumOfTags: {
        $avg: "$numberOfTags"
      }
    }
  }
]
```

> $addFields adds a new field to the document
> $ifNull handles the case where tags is not defined in a document, will set the tags field to an empty array, hence the value of numberOfTags will be 0 for these documents

### Output

```
{
  "_id": null,
  "averageNumOfTags": 3.556
}
```

---

# Question 8: How many users have 'enim' as one of their tags?

```
[
  {
    $match: {
      tags: "enim"
    },
  },
  {
    $count: "userWithEnimTag"
  }
]
```

> $match filters documents

### Output

```
{
  "userWithEnimTag": 62
}
```

---

# Question 9: What are the names and ages of users who are inactive and have 'velit' as a tag?

```
[
  {
    $match: {
      tags: "velit",
      isActive: false
    }
  },
  {
    $project: {
      name: 1,
      age: 1
    }
  }
]
```

> $project gets fields you are looking for, setting a value of 1 means we want the fields name and age with each document

### Output

```
{
  "_id": {
    "$oid": "658f607d2394385bd0a16b9e"
  },
  "name": "Aurelia Gonzales",
  "age": 20
},
{
  "_id": {
    "$oid": "658f607d2394385bd0a16bbc"
  },
  "name": "Hahn Pope",
  "age": 21
},
...,
{
  "_id": {
    "$oid": "658f607d2394385bd0a16ca8"
  },
  "name": "Ferrell Pollard",
  "age": 23
}
```

---

# Question 10: How many users have a phone number starting with "+1 (940)"?

```
[
  {
    $match: {
      "company.phone": { $regex: /^\+1 \(940\)/ }
    }
  },
  {
    $count: "userWithSpecialPhoneNumber"
  }
]
```

> you can use $regex to filter your documents

### Output

```
{
  "userWithSpecialPhoneNumber": 5
}
```

---

# Question 11: Who are the four most recently  registered users?

```
[
  {
    $sort: {
      registered: -1,
    },
  },
  {
    $limit: 4
  }
]
```

### Output

```
{
  "_id": {
    "$oid": "658f607d2394385bd0a16e60"
  },
  "name": "Stephenson Griffith",
  "registered": {
    "$date": "2018-04-14T03:16:20.000Z"
  },
  "favoriteFruit": "apple"
},
{
  "_id": {
    "$oid": "658f607d2394385bd0a16d51"
  },
  "name": "Sonja Galloway",
  "registered": {
    "$date": "2018-04-11T12:52:12.000Z"
  },
  "favoriteFruit": "strawberry"
},
...,
{
  "_id": {
    "$oid": "658f607d2394385bd0a16e65"
  },
  "name": "Mamie Bradford",
  "registered": {
    "$date": "2018-04-09T04:54:20.000Z"
  },
  "favoriteFruit": "banana"
}
```

---

# Question 12: Categorize users by their favorite fruit.

```
[
  {
    $group: {
      _id: "$favoriteFruit",
      users: { $push: "$name" }
    },
  }
]
```

> $push appends a specified value to an array, here will create an array and push all users associated with each fruit and set it as the value for the key users

### Output

```
{
  "_id": "apple",
  "users": Array (338)
},
{
  "_id": "strawberry",
  "users": Array (323)
},
{
  "_id": "banana",
  "users": Array (339)
}
```

---

# Question 13: How many users have 'ad' as the second tag in their list of tags?

```
[
  {
    $match: {
      "tags.1": "ad"
    }
  }, 
  {
    $count: 'secondTagAd
  }
]
```

> "tags.1" targets index 1 of tags array

### Output

```
{
  "secondTag": 12
}
```

---

# Question 14: Find users who have both 'enim' and 'id' as their tags

```
[
  {
    $match: {
      tags: {$all: ["enim", "id"] }
    }
  }, 

]
```

> $all selects the documents if all the criteria in the array is fulfilled

### Output

```
{
  "_id": {
    "$oid": "658f607d2394385bd0a16b9e"
  },
  "name": "Aurelia Gonzales",
  "tags": Array(5)
},
{
  "_id": {
    "$oid": "658f607d2394385bd0a16cfa"
  },
  "name": "Mcgowan Rosales",
  "tags": Array (4)
},
...,
{
  "_id": {
    "$oid": "658f607d2394385bd0a16f20"
  },
  "name": "Spencer Atkinson",
  "tags": Array (5)
}
```

---

# Question 15: List all companies located in the USA with their corresponding user count

```
[
  {
    $match: {
      "company.location.country": "Italy",
    },
  },
  {
    $group: {
      _id: "$company.title",
      userCount: { $sum: 1 }
    }
  },
]
```

### Output

```
{
  "_id": "SLUMBERIA",
  "userCount": 1
},
{
  "_id": "KNOWLYSIS",
  "userCount": 1
},
...,
{
  "_id": "MELBACOR",
  "userCount": 1
}
```

---
---

# Lookups

**Books Collection**

| _id | title | author_id | genre |
| ---- | ---- | ---- | ---- |
| 1 | "The Great Gatsby" | 100 | "Classic" |
| 2 | "Nineteen Eighty-Four" | 101 | "Dystopian" |
| 3 | "To Kill a Mockingbird" | 102 | "Classic" |
**Authors Collection**

| _id | name | birth_year |
| ---- | ---- | ---- |
| 100 | "F. Scott Fitzgerald" | 1896 |
| 101 | "George Orwell" | 1903 |
| 102 | "Harper Lee" | 1926 |
**method 1**

```
[
  {
    $lookup: {
      from: "authors",
      localField: "author_id",
      foreignField: "_id",
      as: "author_details",
    },
  },
  {
    $addFields: {
      author_details: {
        $first: "$author_details",
      },
    },
  },
]
```

> $lookup matches fields between two collections and adds it as a new field, the new field will be an array by default

> $addFields overwrites the author_details field so it will be the first item in the array so it would be easier to work with, however this discards the possibility multiple author documents could be associated with one book.

### Output

```
{
  "_id": 1,
  "title": "The Great Gatsby",
  "author_id": 100,
  "genre": "Classic",
  "author_details": {
    "_id": 100,
    "name": "F. Scott Fitzgerald",
    "birth_year": 1896
  }
},
{
  "_id": 2,
  "title": "Nineteen Eighty-Four",
  "author_id": 101,
  "genre": "Dystopian",
  "author_details": {
    "_id": 101,
    "name": "George Orwell",
    "birth_year": 1903
  }

{
  "_id": 3,
  "title": "To Kill a Mockingbird",
  "author_id": 102,
  "genre": "Classic",
  "author_details": {
    "_id": 102,
    "name": "Harper Lee",
    "birth_year": 1926
  }
}
```

**method 2** (more readable)

```
[
  {
    $lookup: {
      from: "authors",
      localField: "author_id",
      foreignField: "_id",
      as: "author_details",
    },
  },
  {
    $addFields: {
      author_details: {
       $arrayElemAt: ["$author_details", 0]
      },
    },
  },
]
```

> $arrayElemAt takes in an array with two parameters, the array field desired, and the index of the array wanted
### Output

```
{
  "_id": 1,
  "title": "The Great Gatsby",
  "author_id": 100,
  "genre": "Classic",
  "author_details": {
    "_id": 100,
    "name": "F. Scott Fitzgerald",
    "birth_year": 1896
  }
},
{
  "_id": 2,
  "title": "Nineteen Eighty-Four",
  "author_id": 101,
  "genre": "Dystopian",
  "author_details": {
    "_id": 101,
    "name": "George Orwell",
    "birth_year": 1903
  }

{
  "_id": 3,
  "title": "To Kill a Mockingbird",
  "author_id": 102,
  "genre": "Classic",
  "author_details": {
    "_id": 102,
    "name": "Harper Lee",
    "birth_year": 1926
  }
}
