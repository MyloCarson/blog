## Checking if string is an ObjectId in MongoDB using Mongoose

# Problem
Using mongoose.Types.ObjectId.isValid to check if an ObjecId is valid and not a random string will return TRUE for any string of 12 bytes, this could be catastrophic.

![Screenshot 2020-07-08 at 6.18.15 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1594228710038/w2YSOoV_8.png)
# Solution
To solve this, I suggest you create a new ObjectId using the string, then compare the results. ObjectId will always return thesame value if its a true

![Screenshot 2020-07-08 at 6.25.38 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1594229165528/_mc2lD98f.png)
