<template>
    <div id="app">
        <a href="https://github.com/HYFE/beer-ui">
            <img style="position: absolute; top: 0; right: 0; border: 0;width:auto"
                 src="https://camo.githubusercontent.com/38ef81f8aca64bb9a64448d0d70f1308ef5341ab/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f6461726b626c75655f3132313632312e706e67"
                 alt="Fork me on GitHub"
                 data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_darkblue_121621.png">
        </a>
        <v-header :title="title"
                  :version="version"></v-header>
        <main class="docs-container docs-main">
            <aside class="docs-aside">
                <ui-list exact>
                    <ui-listitem :to="{ name: 'home' }">开始</ui-listitem>
                    <ui-listitem :to="{ name: 'contributing' }">贡献</ui-listitem>
                    <ui-listitem :to="{ name: 'icon' }">图标</ui-listitem>
                    <ui-listitem :to="{ name: 'transition' }">过渡</ui-listitem>
                    <ui-treeitem v-for="item in trees"
                                 :key="item.id"
                                 :tree="item"
                                 :isHighlight="highlightNode"
                                 @nodeClick="clickNode">
                        <template slot-scope="{ node }">{{node.text}}</template>
                    </ui-treeitem>
                </ui-list>
            </aside>
            <router-view class="docs-body" />
        </main>
    </div>
</template>
<script>
import vHeader from './components/header'
import { components, directives } from './router/routes'

export default {
    name: 'app',
    components: {
        vHeader
    },
    data () {
        return {
            comps: components.sort((a, b) => a.name.localeCompare(b.name)),
            direcs: directives.sort((a, b) => a.name.localeCompare(b.name)),
            title: 'BeerUI',
            version: ''
        }
    },
    computed: {
        trees () {
            return [{
                id: 'components',
                text: '组件',
                children: this.comps.map(item => ({
                    id: item.name,
                    text: `${item.name} ${item.text}`
                }))
            }, {
                id: 'directives',
                text: '指令',
                children: this.direcs.map(item => ({
                    id: item.name,
                    text: `${item.name} ${item.text}`
                }))
            }]
        }
    },
    methods: {
        clickNode(e, node) {
            if(!node.children) {
                this.$router.push({ name: node.id })
            }
        },
        highlightNode(node) {
            return node.id === this.$route.name
        }
    },
    created () {
        const [title, version] = document.title.split('-')
        this.title = title.trim()
        this.version = version.trim()
    }
}
</script>
<style lang="less">
html,
body {
    height: 100%;
    min-height: 100%;
}

body {
    margin: 0;
    color: #4a4a4a;
    font-size: 14px;
    background: #f0f0f0;
}

.docs- {
    &container {
        width: 90%;
        margin: 0 auto;
    }
    &main {
        display: flex;
        margin-bottom: 30px;
        background: #fff;
        border-radius: 3px;
        box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
    }
    &aside {
        width: 18%;
        min-height: 85vh;
        padding: 8px 0;
        border-right: 1px solid #e8e8e8;
        .ui-treeitem-inner {
            text-transform: capitalize;
        }
    }
    &body {
        flex: 1;
        padding: 25px 30px;
        overflow: hidden;
    }
}
</style>
