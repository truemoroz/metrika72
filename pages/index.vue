<template>
  <el-container>
    <el-header style="border: 1px solid gray">
      header
    </el-header>
    <el-container>
      <el-aside class="hidden-xs-only" style="border: 1px solid gray" width="200px">
        боковое меню
      </el-aside>
      <el-main>
        основная страница
        <el-table
          :data="items"
        >
          <el-table-column prop="id" label="id" min-width="80" />
          <el-table-column prop="params.type" label="тип" min-width="90" />
          <el-table-column prop="complex.title" label="ЖК" />
          <!--          <el-table-column :prop="`${params.rooms}-комн. ${params.category}`" label="Категория" />-->
          <el-table-column label="Категория">
            <template slot-scope="scope">
              {{ scope.row.params.rooms }}-комн. {{ scope.row.params.category }}
            </template>
          </el-table-column>
          <el-table-column prop="params.area" label="Кв.м" width="60" />
          <el-table-column label="Этаж" width="60">
            <template slot-scope="scope">
              {{ scope.row.params.floor }}/{{ scope.row.params.floors }}
            </template>
          </el-table-column>
          <el-table-column prop="user.phone" label="Телефон" />
          <el-table-column prop="managerInfo.name" label="Менеджер" />
          <el-table-column label="Фото/Описание" min-width="110">
            <template slot-scope="scope">
              <el-popover
                v-show="scope.row.files.length !== 0"
                placement="bottom"
                width="400"
                trigger="click"
              >
                <el-row>
                  <el-col v-for="(item, index) of scope.row.files.map(file => file.resize.small)" :key="index" :span="8">
                    <el-image
                      style="width: 100px; height: 100px"
                      :preview-src-list="scope.row.files.map(file => file.resize.origin)"
                      :src="item"
                      fit="fit"
                    >
                      <div slot="placeholder" class="image-slot">
                        Loading<span class="dot">...</span>
                      </div>
                    </el-image>
                  </el-col>
                </el-row>
                <el-button slot="reference" size="mini">
                  фото
                </el-button>
              </el-popover>
              <el-popover
                placement="bottom"
                width="400"
                trigger="click"
              >
                {{ scope.row.params.description }}
                <el-button slot="reference" size="mini">
                  описание
                </el-button>
              </el-popover>
            </template>
          </el-table-column>
          <el-table-column label="Адрес" min-width="100">
            <template slot-scope="scope">
              {{ scope.row.params.city }}, {{ scope.row.params.street }}, {{ scope.row.params.house }}
              <el-popover
                v-show="scope.row.files.length !== 0"
                placement="bottom"
                width="50"
                label="кв"
                :content="scope.row.params.flat"
                trigger="click"
              >
                <el-button slot="reference" size="mini">
                  кв
                </el-button>
              </el-popover>
              <!--              , {{ scope.row.params.flat }}-->
            </template>
          </el-table-column>
          <el-table-column label="Стоимость, р (р/м2)">
            <template slot-scope="scope">
              {{ scope.row.params.price }} ({{ Math.round(scope.row.params.price / scope.row.params.area) }})
            </template>
          </el-table-column>
        </el-table>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data () {
    return {
      items: []
    }
  },
  async fetch () {
    this.items = await this.$axios.$get('/data.json')
  }
}
</script>
