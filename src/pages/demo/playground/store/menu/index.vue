<template>
  <d2-container type="ghost">
    <template slot="header">菜单控制试验台</template>
    <el-row :gutter="20" class="d2-mt d2-mb">
      <el-col :span="12">
        <el-card shadow="never" class="d2-card">
          <template slot="header">顶栏菜单</template>
          <el-button-group class="d2-mb">
            <el-button @click="handleMenuHeaderSet">设置空菜单</el-button>
            <el-button @click="menuHeaderReset">恢复</el-button>
          </el-button-group>
          <div style="height: 400px; overflow: auto;">
            <d2-highlight :code="JSON.stringify(menuHeader, null, 2)"/>
            <!-- <tree-view
              class="tree-view-small"
              :data="menuHeader"
              :options="{
                rootObjectKey: 'menuHeader',
                maxDepth: 1
              }"/> -->
          </div>
        </el-card>
      </el-col>
      <el-col :span="12">
        <el-card shadow="never" class="d2-card">
          <template slot="header">侧栏菜单</template>
          <el-button-group class="d2-mb">
            <el-button @click="handleMenuAsideSet">设置空菜单</el-button>
            <el-button @click="menuAsideReset">恢复</el-button>
          </el-button-group>
          <div style="height: 400px; overflow: auto;">
            <d2-highlight :code="JSON.stringify(menuAside, null, 2)"/>
            <!-- <tree-view
              class="tree-view-small"
              :data="menuAside"
              :options="{
                rootObjectKey: 'menuAside',
                maxDepth: 1
              }"/> -->
          </div>
        </el-card>
      </el-col>
    </el-row>
  </d2-container>
</template>

<script>
import clonedeep from 'lodash.clonedeep'
import { mapState, mapMutations } from 'vuex'
export default {
  data () {
    return {
      menuEmpty: [
        {
          title: '空菜单演示',
          icon: 'folder-o',
          children: [
            {
              title: '空菜单 1',
              children: [
                { title: '空菜单 1-1' },
                { title: '空菜单 1-2' }
              ]
            },
            { title: '空菜单 2' },
            { title: '空菜单 3' }
          ]
        }
      ],
      menuHeaderChanged: false,
      menuAsideChanged: false,
      menuHeaderBak: [],
      menuAsideBak: []
    }
  },
  computed: {
    ...mapState({
      menuHeader: state => state.d2admin.menuHeader,
      menuAside: state => state.d2admin.menuAside
    })
  },
  created () {
    this.menuHeaderBak = clonedeep(this.menuHeader)
    this.menuAsideBak = clonedeep(this.menuAside)
  },
  beforeDestroy () {
    if (this.menuHeaderChanged && this.menuAsideChanged) {
      this.d2adminMenuHeaderSet(this.menuHeaderBak)
      this.d2adminMenuAsideSet(this.menuAsideBak)
      this.$notify({
        title: '菜单恢复',
        message: '对侧边栏和顶栏菜单的修改已经复原',
        type: 'success'
      })
      return
    }
    if (this.menuHeaderChanged) {
      this.d2adminMenuHeaderSet(this.menuHeaderBak)
      this.$notify({
        title: '菜单恢复',
        message: '对顶栏菜单的修改已经复原',
        type: 'success'
      })
      return
    }
    if (this.menuAsideChanged) {
      this.d2adminMenuAsideSet(this.menuAsideBak)
      this.$notify({
        title: '菜单恢复',
        message: '对侧边栏菜单的修改已经复原',
        type: 'success'
      })
    }
  },
  methods: {
    ...mapMutations([
      'd2adminMenuHeaderSet',
      'd2adminMenuAsideSet'
    ]),
    /**
     * 修改顶栏菜单
     */
    handleMenuHeaderSet () {
      this.menuHeaderChanged = true
      this.d2adminMenuHeaderSet(this.menuEmpty)
      this.$notify({
        title: '菜单修改',
        message: '对顶栏菜单的修改已经生效',
        type: 'success'
      })
    },
    /**
     * 修改侧边栏菜单
     */
    handleMenuAsideSet () {
      this.menuAsideChanged = true
      this.d2adminMenuAsideSet(this.menuEmpty)
      this.$notify({
        title: '菜单修改',
        message: '对侧边栏菜单的修改已经生效',
        type: 'success'
      })
    },
    /**
     * 恢复顶栏菜单
     */
    menuHeaderReset () {
      this.d2adminMenuHeaderSet(this.menuHeaderBak)
      this.$notify({
        title: '菜单恢复',
        message: '对顶栏菜单的修改已经复原',
        type: 'success'
      })
    },
    /**
     * 恢复侧边栏菜单
     */
    menuAsideReset () {
      this.d2adminMenuAsideSet(this.menuAsideBak)
      this.$notify({
        title: '菜单恢复',
        message: '对侧边栏菜单的修改已经复原',
        type: 'success'
      })
    }
  }
}
</script>
