<template>
  <div>
    <el-tabs v-model="tab" type="card">
      <el-tab-pane label="Basic" name="basic">
        <project-basic :projectID="projectID"/>
      </el-tab-pane>
      <el-tab-pane label="Sprint" name="sprint">
        <project-sprint :projectID="projectID"/>
      </el-tab-pane>
      <el-tab-pane label="Require" name="require">
        <project-require :projectID="projectID"/>
      </el-tab-pane>
      <el-tab-pane label="Progress" name="progress">
        <project-progress :projectID="projectID"/>
      </el-tab-pane>
      <el-tab-pane label="Defect" name="defect">
        <project-defect :projectID="projectID"/>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
import ProjectRequire from './Require/Require.vue'
import ProjectSprint from './Sprint/Sprint.vue'
import ProjectProgress from './Progress/Progress.vue'
import ProjectDefect from './Defect.vue'
import ProjectBasic from './Basic/Basic.vue'

/* 普通情况下，组件使用 props 传参
 * 本组件由于用于 route-view，无法传动态参数（静态参数可传，但这里需要动态参数）
 * 故使用全局变量传参
 * 下属组件不用路由，没有 route-view 情形，故不要使用全局变量，请使用 props 传参
 */
export default{
  name: 'Project',
  components: {
    'project-sprint': ProjectSprint,
    'project-require': ProjectRequire,
    'project-basic': ProjectBasic,
    'project-defect': ProjectDefect,
    'project-progress': ProjectProgress
  },
  data () {
    return {
      tab: 'basic',
      projectID: ''
    }
  },
  watch: {
    // 若路由路径变化，从全局变量刷新 projectID
    $route () {
      this.projectID = this._GLOBAL.projectID
    }
  }
}
</script>

<style>
</style>
