# Axis

The picture below reveals the composition of our axes: 

![](../../.gitbook/assets/group-4%20%284%29.png)

## The Axis Configuration

### Close the axis

#### `chart.axis(false)` 

Close all axes.

#### `chart.axis(field, false)`

* `field`: String type, the data field name corresponding to the axis

 Do not render the axis corresponding to the field.

### Configure Axis

#### `chart.axis(field, config)`

* `field`: String type, the data field name corresponding to the axis
* `config`: Object type,  configuration for the axis. It can be used to configure each component of the axis: include label, line, tick, tickLine and grid.The properties included are as follows:

| **Name** | **Type** | **Default** | **Description** |
| :--- | :--- | :--- | :--- |
| `line` | Object / null | `{ stroke: '#e8e8e8', lineWidth: 1 }` | The axis line's configuration. **The axis line is not displayed when `line` set to `null`**. It supports all html5 canvas's line attributes, see [canvas](https://antv.gitbook.io/f2/api/canvas-api-in-f2) for more details. |
| `labelOffset` | Number | `7.5` | The distance between the axis label and axis line |
| `grid` | Object/Function/null | `{ type: 'line', stroke: '#e8e8e8', lineWidth: 1, lineDash: [ 2 ] }` | The axis grid's configuration. The grid is not displayed when `grid` is set to `null`. It supports all html5 canvas's line attributes, see [canvas](https://antv.gitbook.io/f2/api/canvas-api-in-f2) for more details.If gird is a function type, see [callback](https://antv.gitbook.io/f2/api/axis#creating-custom-gird) |
| `tickLine` | Object/null | `null` | The configuration for style of the tickLine in axis. The tickLine of axis is not displayed when `tickLine` is set to `null`. It supports all html5 canvas's line attributes, see [canvas](https://antv.gitbook.io/f2/api/canvas-api-in-f2) for more details. |
| `label` | Object/Function/null | `{ fill: '#808080', fontSize: 10 }` | Configuration for label of the axis. The labek of the axis is not displayed when `label` is set to `null`. It supports all  html5 canvas's attributes, see [canvas](https://antv.gitbook.io/f2/api/canvas-api-in-f2) for more details.If label is a function type, see [callback](https://antv.gitbook.io/f2/api/axis#creating-custom-label) |
| `position` | String | `'left'` | Configuration for the position of **the Y-axis**. X-axis defaults to 'bottom', the position of y-axis can be set to 'left' or 'right'. |
| `top` | Boolean | `false` | Used to adjust the layer level. `true` represents the top layer level, `false` means the bottom layer level. |

**`line`, `grid`, `label`, `tickLine` all have a property `top` which is used to adjust the display layer, `true` represents the top layer, `false` means the bottom layer level.**

```javascript
chart.axis('xFieldName', {
  line: {
    top: true
  }
});
```

**Note: When grid and label are callback functions, the return values must be an object.**

```javascript
chart.axis('your data field name', {
  line: {
    lineWidth: 1, 
    stroke: '#ccc' 
  },
  labelOffset: 20, 
  tickLine: {
    lineWidth: 1,
    stroke: '#ccc',
    length: 5
  },
  // highlight for grid line at 0%
  grid: (text, index, total) => {
    if(text === '0%') {
      return {
        stroke: '#efefef'
      };
    }
    return {
      stroke: '#f7f7f7'
    }
  },
  label: (text, index, total) => {
    const cfg = {
      textAlign: 'center'
    };
    if (index === 0) {
      cfg.textAlign = 'start';
    }
    if (index > 0 && index === total - 1) {
      cfg.textAlign = 'end';
    }
    cfg.text = text + '%';  // cfg.text supports text formatting
    return cfg;
  }
});
```

#### **Creating Custom Label** 

If you want to change the axis label to include information about the data type, you can define a callback function for label. In the following example, every label of the Y axis would be formatted as a percentage, and if the value is greater than zero, the font color will be red, equal to zero is black, and less than zero is green.

If the callback returns `null` or `undefined` the label and the associated grid line will be hidden.

See detailed demo: [here](https://antv.alipay.com/zh-cn/f2/3.x/demo/axis/label-callback.html).

```javascript
chart.axis('value', {
    /**
     * @param  {String} text the text string of label
     * @param  {number} index index order
     * @param  {number} total the total numbers of label
     * @return {Object} cfg return the configuration object
     */
    label: function label(text, index, total) {
      var number = parseInt(text);
      var cfg = {};
      if (number > 0) {
        cfg.text = '+' + text;
        cfg.fill = '#F5222D';
      } else if (number === 0) {
        cfg.fill = '#000';
        cfg.fontWeight = 'bold';
      } else {
        cfg.fill = '#52C41A';
      }
      return cfg;
    }
  });

```

![](../../.gitbook/assets/image%20%2817%29.png)

#### **Creating Custom Grid** 

Same as label. The following example will highlight the grid line which the value is zero.

See detailed demo: [here](https://antv.alipay.com/zh-cn/f2/3.x/demo/area/with-negative.html).

```javascript
chart.axis('value', {
  /**
   * @param  {String} text the text string of grid
   * @param  {number} index index order
   * @param  {number} total the total numbers of grid
   * @return {Object} cfg return the configuration object
   */
  grid: function grid(text, index, total) {
    if (text === '0') {
      return {
        lineDash: null,
        lineWidth: 1
      };
    }
  }
});
```

![](../../.gitbook/assets/image%20%2812%29.png)

### More Axis Configuration DemosD

| Demo |  |
| :--- | :--- |
| [label, break line](https://antv.alipay.com/zh-cn/f2/3.x/demo/component/axis-break-line.html) | ![](https://gw.alipayobjects.com/zos/rmsportal/DEwVBFoGLbnMrwHxauyp.png) |
| [label rotate](https://antv.alipay.com/zh-cn/f2/3.x/demo/component/axis-rotate.html) | ![](https://gw.alipayobjects.com/zos/rmsportal/aZQMEqhJsZrHBPVvfwVu.png) |
| [label callback](https://antv.alipay.com/zh-cn/f2/3.x/demo/component/axis-label-callback.html) | ![](https://gw.alipayobjects.com/zos/rmsportal/JNURaLRrBdyAFOgatkwO.png) |
| [grid style configuration](https://antv.alipay.com/zh-cn/f2/3.x/demo/component/axis-grid.html) | ![](https://gw.alipayobjects.com/zos/rmsportal/WgyBJAgRVIwsjaIyPhvA.png) |
| [grid callback](https://antv.alipay.com/zh-cn/f2/3.x/demo/component/axis-grid-callback.html) | ![](https://gw.alipayobjects.com/zos/rmsportal/dWXDCtnpVQFhvhtgSmWy.png) |
| [circle grid](https://antv.alipay.com/zh-cn/f2/3.x/demo/component/axis-circle-grid.html) | ![](https://gw.alipayobjects.com/zos/rmsportal/CnTYvcQBFcUeWmcKutse.png) |
| [iconfont label](https://antv.alipay.com/zh-cn/f2/3.x/demo/component/axis-iconfont.html) | ![](https://gw.alipayobjects.com/zos/rmsportal/wBAMqyEGjiKXvVfkAzSr.png) |

