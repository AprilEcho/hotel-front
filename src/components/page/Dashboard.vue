<template>
  <div>
    <el-row :gutter="20">
      <el-col :span="16">
        <el-row :gutter="20" class="mgb20">
          <el-col :span="8">
            <el-card shadow="hover" :body-style="{padding: '0px'}">
              <div class="grid-content grid-con-1">
                <i class="el-icon-lx-people grid-con-icon"></i>
                <div class="grid-cont-right">
                  <div class="grid-num">{{state.length}}</div>
                  <div>用户入住总量</div>
                </div>
              </div>
            </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="hover" :body-style="{padding: '0px'}">
              <div class="grid-content grid-con-2">
                <i class="el-icon-lx-notice grid-con-icon"></i>
                <div class="grid-cont-right">
                  <div class="grid-num">{{nullRoom.length}}</div>
                  <div>房间剩余总量</div>
                </div>
              </div>
            </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="hover" :body-style="{padding: '0px'}">
              <div class="grid-content grid-con-3">
                <i class="el-icon-lx-goods grid-con-icon"></i>
                <div class="grid-cont-right">
                  <div class="grid-num">{{bookRoom.length}}</div>
                  <div>房间预定总量</div>
                </div>
              </div>
            </el-card>
          </el-col>
        </el-row>
        <el-card shadow="hover" style="height:403px;">
          <div slot="header" class="clearfix">
            <span>待办事项</span>
            <el-button style="float: right; padding: 3px 0" type="text" @click="addtodo()">添加</el-button>
          </div>
          <el-table :show-header="false" :data="todoList" style="width:100%;">
            <el-table-column width="40">
              <template slot-scope="scope">
                <el-checkbox v-model="scope.row.status"></el-checkbox>
              </template>
            </el-table-column>
            <el-table-column>
              <template slot-scope="scope">
                <div
                  class="todo-item"
                  :class="{'todo-item-del': scope.row.status}"
                >{{ scope.row.title }}
                </div>
              </template>
            </el-table-column>
            <el-table-column width="60">
              <template slot-scope="scope">
                <p>
                  <el-button type="text" class="el-icon-edit" style="color: cornflowerblue"
                             @click="edittodo(scope)"></el-button>
                  <el-button type="text" class="el-icon-delete" style="color: red"
                             @click.native.prevent="deletetodo(scope.$index,todoList)"></el-button>
                </p>
                <!--                                        <i class="el-icon-edit" style="color: cornflowerblue" @click="edittodo(scope)">编辑</i>-->
              </template>
            </el-table-column>
          </el-table>
        </el-card>

      </el-col>
      <el-col :span="8">
        <el-card shadow="hover" class="mgb20" style="height:252px;">
          <div class="user-info">
            <img src="../../assets/img/people.gif" class="user-avator" alt/>
            <div class="user-info-cont">
              <div class="user-info-name">{{ name }}</div>
              <div>{{ role }}</div>
            </div>
          </div>
          <div class="user-info-list">
            登录地点：
            <span>    梅州</span>
          </div>
          <div class="user-info-list">
            当前时间：
            <span>{{nowTime}}</span>
          </div>
        </el-card>

        <el-card shadow="hover" style="height:252px;">
          <div slot="header" class="clearfix">
            <span>房间总量详情</span>
          </div>
          剩余房间量
          <el-progress :percentage="nullRoom.length" color="#42b983"></el-progress>
          <!--                清扫房间量-->
          <!--                <el-progress :percentage="9" color="#f1e05a"></el-progress>-->
          预定房间量
          <el-progress :percentage="bookRoom.length" color="#f56c6c"></el-progress>
          已住房间量
          <el-progress :percentage="state.length"></el-progress>
        </el-card>
      </el-col>

    </el-row>
  </div>
</template>

<script>
  import Schart from 'vue-schart';
  import bus from '../common/bus';

  export default {
    name: 'dashboard',
    data() {
      return {
        tableData: [],
        state: [],
        bookRoom: [],
        nullRoom: [],
        nowTime: '',
        name: localStorage.getItem('ms_username'),
        todoList: [
          {
            title: '001号房需要零食',
            status: true
          },
          {
            title: '002号房需要矿泉水',
            status: false
          },
          {
            title: '003号房需要午餐',
            status: false
          },
          {
            title: '004号房需要晚餐',
            status: false
          }
        ],
        data: [
          {
            name: '2018/09/04',
            value: 1083
          },
          {
            name: '2018/09/05',
            value: 941
          },
          {
            name: '2018/09/06',
            value: 1139
          },
          {
            name: '2018/09/07',
            value: 816
          },
          {
            name: '2018/09/08',
            value: 327
          },
          {
            name: '2018/09/09',
            value: 228
          },
          {
            name: '2018/09/10',
            value: 1065
          }
        ],
        options: {
          type: 'bar',
          title: {
            text: '近一周部分住房量柱状图'
          },
          xRorate: 25,
          labels: ['周一', '周二', '周三', '周四', '周五'],
          datasets: [
            {
              label: '标准大床房',
              data: [234, 278, 270, 190, 230]
            },
            {
              label: '商务双床房',
              data: [164, 178, 190, 135, 160]
            },
            {
              label: '豪华大床房',
              data: [144, 198, 150, 235, 120]
            }
          ]
        },
        options2: {
          type: 'line',
          title: {
            text: '近半年部分住房量趋势图'
          },
          labels: ['6月', '7月', '8月', '9月', '10月'],
          datasets: [
            {
              label: '标准大床房',
              data: [234, 278, 270, 190, 230]
            },
            {
              label: '商务双床房',
              data: [164, 178, 150, 135, 160]
            },
            {
              label: '豪华大床房',
              data: [74, 118, 200, 235, 90]
            }
          ]
        }
      };
    },
    components: {
      Schart
    },
    computed: {
      role() {
        return this.name === 'admin' ? '超级管理员' : '前台管理员';
      }
    },

    created() {
      this.getAllGuest();
      this.getNUllRooms();
      this.getHasBookedRoom();
      this.nowTimes();
    },
    // 挂载完成时
    mounted() {
      this.nowTimes();
    },

    methods: {
      //获取全部用户操作
      getAllGuest() {
        //console.log('token: ' + localStorage.getItem('token'));
        this.$http.get('http://localhost:8081/getAllGuest').then((res) => {
          this.tableData = res.data.data.guestMsgs;
          console.log(res.data.data.guestMsgs);
          console.log('getAllGuest方法执行完毕');
          for (let i = 0; i < this.tableData.length; i++) {
            if (this.tableData[i].state == 1) {
              this.state.length++
            }
          }
        });
      },

      //获取空房间
      getNUllRooms() {
        this.$http.get('http://localhost:8081/getNullRooms').then(res => {
          //  console.log(res);
          if (res.data.data === null) {
            this.$message.info('没有剩余房间了');
          } else {
            console.log(res);
            this.nullRoom = res.data.data.nullRooms;
          }
        });
      },

      //获取已经预定的房间
      getHasBookedRoom() {
        this.$http.get('http://localhost:8081/getHasBookedRoom').then(res => {
          if (res.data.data === null) {
            this.$message.info('已预订房间为空');
          } else {
            console.log(res);
            this.bookRoom = res.data.data.roomMsgs;
          }
        });
      },

      changeDate() {
        const now = new Date().getTime();
        this.data.forEach((item, index) => {
          const date = new Date(now - (6 - index) * 86400000);
          item.name = `${date.getFullYear()}/${date.getMonth() + 1}/${date.getDate()}`;
        });
      },
      //显示当前时间（年月日时分秒）
      timeFormate(timeStamp) {
        let year = new Date(timeStamp).getFullYear();
        let month = new Date(timeStamp).getMonth() + 1 < 10 ? "0" + (new Date(timeStamp).getMonth() + 1) : new Date(timeStamp).getMonth() + 1;
        let date = new Date(timeStamp).getDate() < 10 ? "0" + new Date(timeStamp).getDate() : new Date(timeStamp).getDate();
        let hh = new Date(timeStamp).getHours() < 10 ? "0" + new Date(timeStamp).getHours() : new Date(timeStamp).getHours();
        let mm = new Date(timeStamp).getMinutes() < 10 ? "0" + new Date(timeStamp).getMinutes() : new Date(timeStamp).getMinutes();
        let ss = new Date(timeStamp).getSeconds() < 10 ? "0" + new Date(timeStamp).getSeconds() : new Date(timeStamp).getSeconds();
        this.nowTime = year + "年" + month + "月" + date + "日" + " " + hh + ":" + mm + ':' + ss;
      },
      nowTimes() {
        this.timeFormate(new Date());
        setInterval(this.nowTimes, 1000);
        this.clear()
      },
      clear() {
        clearInterval(this.nowTimes)
        this.nowTimes = null;
      },
      //添加待办事项
      todoEdit(edit, item, scope) {
        if (edit) {
          this.$prompt('编辑待办事项', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
          }).then(({value}) => {
            if (item == 'push') {
              this.todoList.push({title: value, status: false})
            } else {
              scope.row.title = value
            }
          }).catch(() => {
            this.$message({
              type: 'info',
              message: '取消输入'
            });
          });
        }

      },
      addtodo() {
        this.todoEdit(true, 'push', null);
      },
      //编辑待办事项
      edittodo(scope) {
        this.todoEdit(true, '', scope)
      },
      //删除待办事项
      deletetodo(index, rows) {
        rows.splice(index, 1)
      }

    }
  };
</script>


<style scoped>
  .el-row {
    margin-bottom: 20px;
  }

  .grid-content {
    display: flex;
    align-items: center;
    height: 100px;
  }

  .grid-cont-right {
    flex: 1;
    text-align: center;
    font-size: 14px;
    color: #999;
  }

  .grid-num {
    font-size: 30px;
    font-weight: bold;
  }

  .grid-con-icon {
    font-size: 50px;
    width: 100px;
    height: 100px;
    text-align: center;
    line-height: 100px;
    color: #fff;
  }

  .grid-con-1 .grid-con-icon {
    background: rgb(50, 65, 87);
  }

  .grid-con-1 .grid-num {
    color: rgb(50, 65, 87);
  }

  .grid-con-2 .grid-con-icon {
    background: rgb(50, 65, 87);
  }

  .grid-con-2 .grid-num {
    color: rgb(50, 65, 87);
  }

  .grid-con-3 .grid-con-icon {
    background: rgb(50, 65, 87);
  }

  .grid-con-3 .grid-num {
    color: rgb(50, 65, 87);
  }

  .user-info {
    display: flex;
    align-items: center;
    padding-bottom: 20px;
    border-bottom: 2px solid #ccc;
    margin-bottom: 20px;
  }

  .user-avator {
    width: 120px;
    height: 120px;
    border-radius: 50%;
  }

  .user-info-cont {
    padding-left: 50px;
    flex: 1;
    font-size: 14px;
    color: #999;
  }

  .user-info-cont div:first-child {
    font-size: 30px;
    color: #222;
  }

  .user-info-list {
    font-size: 14px;
    color: #999;
    line-height: 25px;
  }

  .user-info-list span {
    margin-left: 40px;
  }

  .mgb20 {
    margin-bottom: 20px;
  }

  .todo-item {
    font-size: 14px;
  }

  .todo-item-del {
    text-decoration: line-through;
    color: #999;
  }

  .schart {
    width: 100%;
    height: 300px;
  }
</style>
