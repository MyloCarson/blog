## Shaved up ~35% off bundle size; How?

Using Nuxtjs, it ships with the webpack bundle analyzer; which helps to give an overview of your bundle. Run the command

``` nuxt build --analyze ```


#### **Before**
![Screenshot 2021-09-24 at 12.48.39.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632672133027/bxu-7h6mN.png)


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632672343717/bDwSzJSiM.png)
The final bundle size is at 420kb ( Not so bad. But can be improved upon)

Few details caught my attention from the first image
- success.svg
- failed.svg

Taking a further look, I found out those two assets constituted ~108kb. 

Damn!!! How did this happen?

As a frontend engineer, I love the idea of being able to manipulate my SVGs, change colours, add some hover effects and so on. In order for me to do this effectively, I have to;
- download the SVG into the project
- make it a Vue component
- import this component everywhere I need it or make it a global component (depending on the use case)
By making it a component, it is more flexible and can be changed based on CSS classes or any logic you'd like to apply. It's all fun when you have to do this for few SVG files but it becomes more of a routine when you have to do this for a large project with lots of SVGs, thus we often tend to use `vue-svg-loader`

Due to the ease of use and zero complexity of `vue-svg-loader`, we tend to abuse it and litter the whole project with it, especially in cases when we can avoid using it.

The idea of using SVG as a component is awesome, but it can take a toll on the bundle size of your project.

#### **Solution:**
- Only make SVG a component if that's the last resort, else use `<img />`, `<picture />` tags
- Move all assets to the cloud using services such as Cloudinary

Here is the bundle analysis after applying these few steps:


![Screenshot 2021-09-26 at 17.25.58.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632673599676/BxBPjTp5S.png)




![Screenshot 2021-09-26 at 17.26.12.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632673602574/yQ_D55w1m.png)


###### Links

- [Webpack bundle analyzer](https://www.npmjs.com/package/webpack-bundle-analyzer)

###### Resources:
- [Debbie O'Brien's "Let's Analyze your webpack bundles with Nuxt"](https://debbie.codes/blog/nuxt-analyze-webpack-bundles/)