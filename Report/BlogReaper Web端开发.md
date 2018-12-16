## BlogReaper Web端开发

### 项目简介

“博客收割者，收割你的博客”

`BlogReaper`是一个在线RSS阅读器，通过添加自己感兴趣的RSS源，来将这些RSS进行分类，并且可以在同一个网站内对它们进行浏览，提升阅读体验。

### 技术栈

- 基本框架：`Vue 2`
- UI组件库：`iview 3`
- 网络库：`Vue Apollo 3`
- 用户授权系统：`Violet 2`

### 小结

我在此次项目的任务是参与Web端的开发，包括部分页面的UI和交互功能的实现，主要依靠iView中提供的UI组件以及Vue的JSX语法。

#### iView

##### 简介

iView 是一套基于 Vue.js 的开源 UI 组件库，主要服务于 PC 界面的中后台产品。

##### 特性

- 高质量、功能丰富
- 友好的 API ，自由灵活地使用空间
- 细致、漂亮的 UI
- 事无巨细的文档
- 可自定义主题



#### JSX

JSX就是Javascript和XML结合的一种格式。

Vue 推荐在绝大多数情况下使用 template 来创建HTML。但在一些场景中需要template具有JavaScript的编译能力，这时就可以使用JSX配合render函数来实现。



以一个管理RSS源的Demo页面为例：

`<temolate>`部分：

```vue
<template>
  <div class="manger-comp">
    <div class="title-box">
      <div class="title-top">Organize Sources</div>
      <div class="title">阅读源管理
        <div class="list-control">
          <Icon class="btn-refresh" type="md-refresh"/>
          <Icon class="btn-refresh" type="ios-more"/>
        </div>
      </div>
    </div>
    <div class="search-box">
      <Input class="search" prefix="ios-search" placeholder="Source Name"/>
      <Button id="unfollow" class="btn" ref="unfollow"><Icon class="btn-icon" type="ios-trash-outline"/><span class="btn-text">UNFOLLOW</span></Button>
      <strong id="selected-text" ref="selectedText">{{selected_num}}</strong>
    </div>
    <Table ref="selection" size="large" class="source-table" :columns="columns" :data="data1"
            @on-selection-change="changeChose"
            @on-select="getTableChosedValue"
            @on-select-all="selectAll"></Table>
  </div>
</template>

<style lang="scss">
.manger-comp {
  text-align: center;
  width: 100%;
  margin: 20px auto;
  display: inline-block;
  .title-box {
    .title-top {
      user-select: none;
      text-align: left;
      max-width: 600px;
      margin: 0 auto 10px auto;
      font-size: 16px;
      color: rgb(185, 184, 184);
    }
    .title {
      user-select: none;
      margin: 0 auto 35px auto;
      max-width: 600px;
      text-align: left;
      font-size: 28px;
      padding-left: 10px;
      font-weight: bold;
      border-left: 6px solid #34a1a5;
      i {
        vertical-align: middle;
        margin-right: 10px;
      }
      .list-control {
        float: right;
        .btn-refresh {
          cursor: pointer;
          color: #757575;
          border-radius: 6px;
          font-size: 24px;
          padding: 8px;
          transition: all 0.4s;
          &:hover {
            background: #f1f1f1;
          }
        }
      }
    }
  }
  .source-table {
    max-width: 600px;
    margin: 15px auto 10px auto;
  }
  .search-box {
    position: relative;
    max-width: 600px;
    margin: 20px auto 0px auto;
    text-align: left;
    .search {
      display: block;
      width: 200px;
    }
  }
}

.source-box {
  margin-bottom: 10px;
  position: relative;
}
.head-image {
  width: 50px;
  height: 50px;
  margin-top: 15px;
  margin-bottom: 10px;
  margin-right: 20px;
}
.source-name {
  display: inline-block;
  position: relative;
  bottom: 30px;
}
.trash-icon {
  font-size: 20px;
}
.btn-icon {
  font-size: 15px;
  margin-right: 10px;
}
.btn-text {
  margin-top: 5px;
}
#selected-text {
  position: relative;
  top: 8px;
  font-size: 15px;
  display: inline-block;
  margin-left: 20px;
  opacity: 0;
}
#unfollow {
  display: inline-block;
  margin-top: 15px;
  opacity: 0;
}
</style>

<script>
export default {
  data () {
    return {
      columns: [
        {
          type: 'selection',
          width: 50,
          align: 'center'
        },
        {
          title: 'Source Name',
          key: 'name',
          width: 280,
          sortable: true,
          render: (h, params) => {
            return h('div', [
              h('img', {
                class: {
                  'head-image': true
                },
                attrs: {
                  src: params.row.img
                }
              }),
              h('strong', {
                class: {
                  'source-name': true
                }
              }, params.row.name)
            ], {
              class: {
                'source-box': true
              }
            })
          }
        },
        {
          title: 'Stories',
          key: 'stories',
          sortable: true
        },
        {
          title: 'Reads',
          key: 'reads',
          sortable: true
        },
        {
          title: ' ',
          key: 'action',
          align: 'center',
          width: 60,
          render: (h, params) => {
            return h('div', [
              h('Icon', {
                class: {
                  'trash-icon': true
                },
                props: {
                  type: 'ios-trash-outline'
                },
                on: {
                  click: () => {
                    this.remove(params.index)
                  }
                }
              }, 'Delete')
            ])
          }
        }
      ],
      data1: [
        {
          name: 'Zhenly',
          stories: 1,
          reads: 2,
          img: 'http://img.hb.aicdn.com/007c910095e3cdc0293cebc1d84b08dbac01cb24159ce-JYDpS7_fw658'
        },
        {
          name: 'test2',
          stories: 2,
          reads: 0,
          img: 'http://img.hb.aicdn.com/aa38a6ac44345f733067c999a74be222fb4d979467660-jOoJUl_fw658'
        },
        {
          name: 'test3',
          stories: 30,
          reads: 1,
          img: 'http://img.hb.aicdn.com/39460095a1be72702a8e104443fef79514fa7e1e4f3c-I19CMi_fw658'
        },
        {
          name: 'test4',
          stories: 6,
          reads: 999,
          img: 'http://img.hb.aicdn.com/c2e0966db1e7971f235019db59a279add0fef99a136e2-Sdjq1A_fw658'
        }
      ],
      selected_num: 'Select 0 of'
    }
  },
  methods: {
    show (index) {
      this.$Modal.info({
        title: 'User Info',
        content: `Name：${this.data1[index].name}<br>Age：${this.data1[index].age}<br>Address：${this.data1[index].address}`
      })
    },
    remove (index) {
      this.data1.splice(index, 1)
    },
    changeChose (selection) {
      console.log(selection)
      this.selected_num = 'Select ' + selection.length + ' of ' + this.data1.length
      if (selection.length === 0) {
        this.$refs.unfollow.$el.style.opacity = 0
        this.$refs.selectedText.style.opacity = 0
      } else {
        this.$refs.unfollow.$el.style.opacity = 1
        this.$refs.selectedText.style.opacity = 1
      }
    },
    getTableChosedValue (selection, row) {

    },
    selectAll (selection) {

    }
  }
}
</script>

```

`CSS`部分：

```vue
<style lang="scss">
.manger-comp {
  text-align: center;
  width: 100%;
  margin: 20px auto;
  display: inline-block;
  .title-box {
    .title-top {
      user-select: none;
      text-align: left;
      max-width: 600px;
      margin: 0 auto 10px auto;
      font-size: 16px;
      color: rgb(185, 184, 184);
    }
    .title {
      user-select: none;
      margin: 0 auto 35px auto;
      max-width: 600px;
      text-align: left;
      font-size: 28px;
      padding-left: 10px;
      font-weight: bold;
      border-left: 6px solid #34a1a5;
      i {
        vertical-align: middle;
        margin-right: 10px;
      }
      .list-control {
        float: right;
        .btn-refresh {
          cursor: pointer;
          color: #757575;
          border-radius: 6px;
          font-size: 24px;
          padding: 8px;
          transition: all 0.4s;
          &:hover {
            background: #f1f1f1;
          }
        }
      }
    }
  }
  .source-table {
    max-width: 600px;
    margin: 15px auto 10px auto;
  }
  .search-box {
    position: relative;
    max-width: 600px;
    margin: 20px auto 0px auto;
    text-align: left;
    .search {
      display: block;
      width: 200px;
    }
  }
}

.source-box {
  margin-bottom: 10px;
  position: relative;
}
.head-image {
  width: 50px;
  height: 50px;
  margin-top: 15px;
  margin-bottom: 10px;
  margin-right: 20px;
}
.source-name {
  display: inline-block;
  position: relative;
  bottom: 30px;
}
.trash-icon {
  font-size: 20px;
}
.btn-icon {
  font-size: 15px;
  margin-right: 10px;
}
.btn-text {
  margin-top: 5px;
}
#selected-text {
  position: relative;
  top: 8px;
  font-size: 15px;
  display: inline-block;
  margin-left: 20px;
  opacity: 0;
}
#unfollow {
  display: inline-block;
  margin-top: 15px;
  opacity: 0;
}
</style>
```

`JavaScript`部分：

```vue
<script>
export default {
  data () {
    return {
      columns: [
        {
          type: 'selection',
          width: 50,
          align: 'center'
        },
        {
          title: 'Source Name',
          key: 'name',
          width: 280,
          sortable: true,
          render: (h, params) => {
            return h('div', [
              h('img', {
                class: {
                  'head-image': true
                },
                attrs: {
                  src: params.row.img
                }
              }),
              h('strong', {
                class: {
                  'source-name': true
                }
              }, params.row.name)
            ], {
              class: {
                'source-box': true
              }
            })
          }
        },
        {
          title: 'Stories',
          key: 'stories',
          sortable: true
        },
        {
          title: 'Reads',
          key: 'reads',
          sortable: true
        },
        {
          title: ' ',
          key: 'action',
          align: 'center',
          width: 60,
          render: (h, params) => {
            return h('div', [
              h('Icon', {
                class: {
                  'trash-icon': true
                },
                props: {
                  type: 'ios-trash-outline'
                },
                on: {
                  click: () => {
                    this.remove(params.index)
                  }
                }
              }, 'Delete')
            ])
          }
        }
      ],
      data1: [
        {
          name: 'Zhenly',
          stories: 1,
          reads: 2,
          img: 'http://img.hb.aicdn.com/007c910095e3cdc0293cebc1d84b08dbac01cb24159ce-JYDpS7_fw658'
        },
        {
          name: 'test2',
          stories: 2,
          reads: 0,
          img: 'http://img.hb.aicdn.com/aa38a6ac44345f733067c999a74be222fb4d979467660-jOoJUl_fw658'
        },
        {
          name: 'test3',
          stories: 30,
          reads: 1,
          img: 'http://img.hb.aicdn.com/39460095a1be72702a8e104443fef79514fa7e1e4f3c-I19CMi_fw658'
        },
        {
          name: 'test4',
          stories: 6,
          reads: 999,
          img: 'http://img.hb.aicdn.com/c2e0966db1e7971f235019db59a279add0fef99a136e2-Sdjq1A_fw658'
        }
      ],
      selected_num: 'Select 0 of'
    }
  },
  methods: {
    show (index) {
      this.$Modal.info({
        title: 'User Info',
        content: `Name：${this.data1[index].name}<br>Age：${this.data1[index].age}<br>Address：${this.data1[index].address}`
      })
    },
    remove (index) {
      this.data1.splice(index, 1)
    },
    changeChose (selection) {
      console.log(selection)
      this.selected_num = 'Select ' + selection.length + ' of ' + this.data1.length
      if (selection.length === 0) {
        this.$refs.unfollow.$el.style.opacity = 0
        this.$refs.selectedText.style.opacity = 0
      } else {
        this.$refs.unfollow.$el.style.opacity = 1
        this.$refs.selectedText.style.opacity = 1
      }
    },
    getTableChosedValue (selection, row) {

    },
    selectAll (selection) {

    }
  }
}
</script>
```



该页面中使用了iView的Table控件实现了一个可交互的表格，包括RSS源数据的显示，RSS源表项的删除，表项排序以及表项多选功能。

控件标签中的`@on-selection-change`、`@on-select`、`@on-select-all`分别指定了当选择的表项发生改变、有表项被选取、表项被全选时触发的方法，这些方法的实现在`export default`的`methods`中定义。

对于表格每一列的设置以及表格填充的数据，通过`:cloumns`和`:data`属性来指定。这里的`:cloumns`指定的是一个数组，其每个元素对应每个表列，包括`title`，`key`，`sortable`等重要属性。其中`title`定义了表头显示的内容，`key`对应于`:data`指定的数据中的同名属性，也即数据中要显示在这一列的内容。`sortable`设置是否可根据该列对表项进行排序。

因为要实现多选功能，所以`:cloumns`中的第一个元素较特殊。这一列用来放置单选框，通过设置属性`type: 'selection'`即可。设置了该属性后，该列对应的表头也会显示一个单选框，用于全选操作。

在默认情况下，每个列都会显示对应表项中相应于`key`的属性，如果要为该内容添加样式或是在表项中添加一些子控件，则需要使用`render`函数来渲染。主要的语法为：

```vue
render:(h,params) => {
    return h('',[])
}
```

即通过"标签+属性"的方式来渲染。每个属性则通过`params ：values`的方式进行赋值。

比如要渲染一个带按钮的表项，可以先加入一个`div`标签，方便布局，然后在其属性中可以放置其他子标签，使用`h('',{})`的语法形式。

其中`class`属性与`v-bind:class`的API相同，通过将相应类名的属性设置为`true`即可将控件定义为该类。

`style`属性与`v-bind:style`的API相同，用于指定样式。

`attrs`则用于设置普通的HTML属性，比如id。

`props`用于设置组件的各种相应的props属性。

事件处理程序则放到`on`中，比如`click: () =>{}`。

最后，如果要在事件处理函数中获取某个指定控件，则需要先给相应控件设置`ref`属性，这样就能通过`this.$refs.XXX`来获得相应控件并访问它们的各种属性。