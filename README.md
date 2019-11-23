# Data Visualization

> Displaying data in the form of a chart will greatly improve readability and reading efficiency

> This example includes a histogram, a line chart, a scatter chart, a heat map, a complex histogram, a preview panel, etc.

# Technology stack

- vue2.x
- vuex _ store public variables, such as color values, etc.
- vue-router _route _
- element-ui _ hungry? Based on the vue2 development component library, this example uses the datePicker_
- echarts _ a rich chart library _
- webpack, ES6, Babel, Stylus...

# Project screenshot

<div align=center><img src="https://github.com/SimonZhangITer/DataVisualization/blob/master/static/img/demo.jpg?raw=true"></div>

# Development

## Componentization

The project is completely developed using componentized ideas. Using vue-router as a route, each page is a component, and each component contains multiple components. As you can see, the title and date filter of various charts are similar formats, so both of them are made into components.

In addition, the checkbox used in the screening of the product was also written into a component by me. Friends who need it can also be stripped and used separately (but the writing is rough ~)

Each chart is a separate component that can also be stripped for use.

## histogram

This project contains a variety of charts, here to pick a "histogram" to say that other icons are implemented in a similar way.

```html
<template>
<div class="multipleColumn">
  <v-header :name="name" :legendArr="legendArr" :myChart="myChart"></v-header>
  <v-filter :myChart="myChart" v-if="myChart._dom"></v-filter>
  <div class="main"></div>
</div>
</template>
```
The page HTML can be seen, a complete icon is composed of three parts:



### v-header
Header components, storage headers, different types of filters, etc.

- name _ chart title _
- legendArr _ Chart contains multiple types _
- myChart _ current chart object _

### v-filter

Filter components, date screening, and screening for different products

- myChart _ current chart object _

V-if="myChart._dom" means to render the filter component after the current chart dom element is rendered.

### main

_ chart body file _

Note that <font color=red>main must specify the width and height, otherwise echarts cannot render dom</font>

### Initialization

Initialization needs to be done in vue's mounted() method, as this ensures that the current page elements have already been loaded.

```javascript
Mounted() {
  // Initialize the echarts instance based on the prepared dom
  this.myChart = echarts.init(document.querySelector('.multipleColumn .main'))
  this.myChart.setOption(this.options) //this.options is the configuration of echarts, details can be viewed on my gitHub
}
```

If you want to execute in the created() method, you need to add

```javascript
This.$nextTick(() => {
  This._init()
})
```

## DashBoard

The dashboard is a preview of all the charts, and there is a one-click animation, here is an explanation of an implementation.

### html

```html
<template lang="html">
  <div class="dashboard">
    <div class="flex-container column">
        <div class="item one" @click="clickChart('1')" style="transform: translate(-22.4%,-33.5%) scale(0.33)">
          <multipleColumn></multipleColumn>
        </div>
        <div class="item two" @click="clickChart('2')" style="transform: translate(-22.4%,0.5%) scale(0.33)">
          <column></column>
        </div>
        <div class="item three" @click="clickChart('3')" style="transform: translate(-22.4%,34.5%) scale(0.33)">
          <v-line></v-line>
        </div>
        <div class="item four active" @click="clickChart('4')" style="transform: translate(43.7%, 0) scale(1)">
          <point></point>
        </div>
    </div>
  </div>
</template>
```
As you can see, here is a Wrapper with four charts, each with a chart component. Switch by dynamically changing the style style.

The overall idea is:

- Use a percentage layout so that you can adapt to a screen that cannot be sized
- Determine the chart display ratio, aspect ratio
- Just do a transform transform to improve performance

### Performance

Regarding performance, here is one more sentence:

I believe that everyone has seen the demo of the online demo. The switching between different charts not only has the position change, but also the size change. Then the size of the transformation we all know is to use the transform scale transformation, but the position of the transformation, use left, top?

Obviously this is not true, but it does achieve results, but it will consume a lot of performance. A CSS property change is equivalent to a thread, then if you use left, top, and transform, then three threads are turned on at the same time, then your computer temperature will rise so quickly.

The correct solution is to continue using transform, using its <font color=red> translate </font>, such as:

```css
Transform: translate(-22.4%,0) scale(0.33)
```

# Conclusion

This project is still a very practical project, and it better uses the componentization idea of ​​vue.

Everyone interested can go and see the code, I hope to help everyone.


## Build Setup

``` bash
# install dependencies
Npm install

# serve with hot reload at localhost:8080
Npm run dev

# build for production with minification
Npm run build
```