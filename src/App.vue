<template>
  <div id='app'>
    <div class='wrapper'>
      <header>
        <input class='tree-search-input' type='text' v-model='searchword' placeholder='search...' @keyup='search'/>
        <button class=' tree-search-btn' type='button' @click='search'>Search</button>
        <br><br>
      </header>
      <div class='content-wrapper'>
        <section class='section dak'>
          <v-tree ref='tree' :data='treeData1' :multiple='true' :tpl='tpl' :halfcheck='true' v-on:node-click="nodeClick" />
        </section>
        <section class='section rkk'>
          <my-grid
            :heroes='gridData'
            :columns='gridColumns'
            :filter-key='searchword'>
          </my-grid>
        </section>
      </div>

    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      gridColumns: ['name', 'description'],
      /* gridData: [
        { name: 'Chuck Norris', description: Infinity },
        { name: 'Bruce Lee', description: 9000 },
        { name: 'Jackie Chan', description: 7000 },
        { name: 'Jet Li', description: 8000 }
      ], */
      searchword: '',
      treeData1: [{
        'title': 'САУО АУРА',
        'expanded': true,
        'children': [
          {
            'title': 'Конструктор процессов',
            'expanded': true,
            'children': [
              {
                'title': 'Процесс'
              },
              {
                'title': 'Событие'
              },
              {
                'title': 'Действие'
              },
              {
                'title': 'Форма документа'
              },
              {
                'title': 'Роль'
              },
              {
                'title': 'Связь'
              },
              {
                'title': 'Логическое правило'
              }
            ]
          },
          {
            'title': 'Конструктор форм документов',
            'children': [
              {
                'title': 'Форма документа'
              },
              {
                'title': 'Реквизит'
              },
              {
                'title': 'Классификатор'
              },
              {
                'title': 'Таблица'
              },
              {
                'title': 'Диаграмма'
              }
            ]
          },
          {
            'title': 'Конструктор интерфейсов',
            'children': [
              {
                'title': 'Кнопка'
              },
              {
                'title': 'Переключатель'
              },
              {
                'title': 'Ссылка'
              },
              {
                'title': 'Коментарий'
              },
              {
                'title': 'Другой объект'
              }
            ]
          },
          {
            'title': 'Автоматический программный агент',
            'children': [
              {
                'title': 'АПА клиент'
              },
              {
                'title': 'АПА Сервер'
              }
            ]
          }
        ]
      }],
      currentNode: ''
    }
  },
  computed: {
    gridData: function () {
      let children = this.currentNode.children
      if (!children || children.length === 0) return []
      let array = []
      children.forEach(function (child, index) {
        let childObj = {
          name: child.title,
          description: `description ${index}`
        }
        array.push(childObj)
      })
      return array
    }
  },
  methods: {
    // tpl (node, ctx, parent, index, props) {
    tpl (...args) {
      let {0: node, 2: parent, 3: index} = args
      let titleClass = node.selected ? 'node-title node-selected' : 'node-title'
      if (node.searched) titleClass += ' node-searched'
      return <span>
        <button class='treebtn1' onClick={() => this.$refs.tree.addNode(node, {title: 'sync node'})}>+</button>
        <span class={titleClass} domPropsInnerHTML={node.title} onClick={() => {
          this.$refs.tree.nodeSelected(node)
        }}></span>
        <button class='treebtn2' onClick={() => this.asyncLoad(node)}>async</button>
        <button class='treebtn3' onClick={() => this.$refs.tree.delNode(node, parent, index)}>delete</button>
      </span>
    },
    async asyncLoad (node) {
      this.$set(node, 'loading', true)
      let pro = new Promise(resolve => {
        setTimeout(resolve, 2000, ['async node1', 'async node2'])
      })
      this.$refs.tree1.addNodes(node, await pro)
      this.$set(node, 'loading', false)
    },
    search () {
      this.$refs.tree.searchNodes(this.searchword)
    },
    nodeClick (node) {
      this.currentNode = node.selected ? node : ''
      return this.currentNode
    }
  }
}
</script>

<style >
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background: lightblue;
}
.wrapper {
  width: 100%;
  height: 100vh;
  padding-top: 60px;
}
.content-wrapper{
  display: flex;
}
.section {
  width:  50%;
  height: 90vh;
  border: 2px solid blue;
  margin: 5px;
}
.treebtn1, .treebtn2, .treebtn3 {
  display: none;
}
.halo-tree .tree-node-el {
    background: transparent !important;
}
</style>
