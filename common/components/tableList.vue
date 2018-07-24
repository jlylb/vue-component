<template>

<div>

<div v-if='formColumns.length > 0'>
 <search-form :form-columns='formColumns' @search-form='handleFilter'></search-form>
</div>

  <el-table
    :data='data' 
    stripe
    style="width: 100%">
    <el-table-column v-for='item in columns' :key='item.prop'
      v-bind="item">
    </el-table-column>
    <el-table-column label="操作">
      <template slot-scope="scope">
        <el-button
          size="mini"
          type="primary"
          @click="handleEdit(scope.row)">编辑</el-button>
        <el-button
          size="mini"
          type="danger"
          @click="handleDelete(scope.row)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>

  <div class='page-container'>
    <el-pagination 
    background 
    @size-change="handleSizeChange" 
    @current-change="handleCurrentChange" 
    :current-page="search.page" 
    :page-sizes="pageSizes" 
    :page-size="search.pageSize" 
    layout="total, sizes, prev, pager, next, jumper" 
    :total="total">
    </el-pagination>
   </div>

</div>  
</template>

<script>
import lists from '../mixins/lists'
import SearchForm from './searchForm'
export default {
    mixins: [ lists ],
    components: { SearchForm },
    props: {
        customColumns: {
            type: Object,
            default() {
                return {}
            }
        },
        formColumns: {
            type: Array,
            default() {
                return []
            } 
        },
        tableData: {
            type: Array,
            default() {
                return []
            } 
        },
        total: {
            type: Number,
            default() {
                return 0
            }
        },
        pageSizes: {
            type: Array,
            default() {
                return [ 10, 20, 30, 50 ]
            }
        },
        searchData: {
            type: Object,
            default() {
                return {
                    page: 1,
                    pageSize: 10
                }
            }
        }
    },
    watch: {
        tableData(newval) {
            this.data = newval
            this.setColumns()
        }
    }
}
</script>
<style lang="scss">
.page-container{
    margin: 10px 0;
}
</style>