## Structuring Reactjs / Nextjs Applications

In a collaborative environment, while building web applications, the overall project structure and directory management could be a great deal to how your team mates would love working with you on the project and also how it makes them think less navigating your project. Having a signature project structure would also make it fun to work continuously with folks on multiple projects.

In the context of React, I often find a lot of beginners  creating dangling components. It is quite easy to fall into this trap if you find yourself mostly working alone, but it becomes a whole new world when someone else needs to read your code when you are not available and they can't proceed because the project structure is pretty bad.

Whenever I think of React, what always come to mind is "building re-usable user interfaces". Having this in mind keeps me in constant checks when breaking down a design  into bunch of re-useable components. By so doing, anyone can get on my projects with little or no supervision from me and they can have some level of understanding on how navigate.

## My typical React app structure.

![Screenshot 2020-08-22 at 17.36.53.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1598114233660/TOx-PJbSB.png)
This could be called the flat structure. Mere looking at it, you can have a conceptual overview of where to go to find what.
I am mostly concerned with the 'src' directory, because this is where most of the work go down.

### - assets  
![Screenshot 2020-08-22 at 17.44.05.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1598114664927/FW5GyOEd9.png)
Assets contains styles sheets and any  asset that can be used in the project that doesn't URL reference.
Are you curious about the 'scss' directory? I have a well explained detailing [here](https://github.com/MyloCarson/scss-boilerplate).

### - components

![Screenshot 2020-08-22 at 17.51.48.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1598115133204/zhGX1fre_.png)

Components directory contains all UI elements e.g button, form elements, layouts, etc.
As a frontend engineer, when you closely observe a designs across the project, you certainly will note the following

- UI elements that are used in multiple places (sometimes with similar or slightly different) e.g buttons, inputs, avatar, links, etc
- Re-occuring SVG elements, etc. edit-icon, delete-icon
- At least one or two common layouts. Think of the layout as an overall housing for all these UI elements.
    ![screencapture-retroly-io-2020-08-22-18_08_21.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1598116193641/KUDa_vle3.png)
    >Retroly.io's landing page - Layout 1

    ![Screenshot 2020-08-22 at 18.05.44.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1598116013146/4uD3NRxp3.png)
    >Retroly.io's dasboard page - Layout 2

   Closely studying "Layout 1", it has a footer, different navigation links make up on the Header 
   and also different background when compared to Layout 2.

   With the above recurring patterns, I have come up with a breakdown of all components into 

   1. blocks : components are just any react component (controlled & uncontrolled) e.g Form, 
       TextArea, Input, Card, etc
   2. shared: components that are shared among multiple layouts e.g Footer, Header, Button, 
      Avatar, Progress, etc
   3. layouts:  components here are High order components that houses all other building blocks 
     e.g DefaultLayout, AlternateLayout, AdminLayout, etc.

     ![code.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1598117940067/nTl-SZ_oB.png)
   4. vectors: components are mostly svgs e.g Icons, logos, illustrations e.t.c. I find it easier to 
   re-use SVG elements by making them React components
 
    ![code.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1598117718544/uZ3rIQqAD.png)
   Bhanu Teja Pachipulusu has an article with a different approach [here](https://blog.bhanuteja.dev/how-to-import-svgs-into-your-nextjs-project-ckdx1m6jf033ajas1dzdh2zmp)  

    > Using React Components as re-useable SVG.

    ![Screenshot 2020-08-22 at 19.00.58.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1598119409823/SQGd-cO_m.png)
   > vector folder structure
  
### - constants 
   ![constants.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1598118450837/DMwMVAouI.png)
  Constants directory contains a single file with globally accessible constants like Redux actions, statuses, enums, 
   etc.

### - services
   Services directory contains resources that can be used as service to supply data to your application e.g API 
   Client, dummy json data, etc. Using Axios in React, you can declare a reusable Axios Client.

   ![api-client.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1598118791339/PXT90BpmZ.png)
   > A basic Axios client.

### - utils:

Utils directory contains utility or helper functions. The functions here are functions that can be used anywhere in the application. Example a function that returns unique id that can be used as key for react components, a function that return the user token (this could be helpful for authentication), or a function that returns user's data.

![util.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1598120845164/Nj8fQ6zj0.png)

## Additions
One fun fact is that this folder structure works well with concepts like CSS-modules, Atomic CSS, Block-Element-Modifier (BEM).


## Links
Duomly has an awesome read on more approaches too [here](https://dev.to/duomly/you-have-to-read-this-before-you-will-plan-the-structure-of-your-next-frontend-application-2g64)
