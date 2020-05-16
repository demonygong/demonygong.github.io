---
title: iview对接
date: 2019-10-31 10:52:46
categories:
- iview
---

iView 是一套基于 Vue.js 的 UI 组件库，主要服务于 PC 界面的中后台产品。
* 高质量、功能丰富；
* 友好的 API ，自由灵活地使用空间；
* 使用单文件的 Vue 组件化开发模式。
<!--more-->

![funnel](/img/iview.png)

## 安装 #
```
//  使用 npm
$ npm install iview@1.0.1 --save
//  或使用 <script> 全局引用
<script type="text/javascript" src="iview.min.js"></script>
```
## 表格数据
	
```
	  <Table stripe :columns="columns1" :data="list"></Table>
	  <div class="block" style="text-align: center;margin: 20px auto;">
	      <Page :total="total" 		// 分页
	        :page-size="pageSize" 
	        @on-change="changepage" 
	        show-total  
	        show-elevator 
	        :current="pageNum" />
	  </div>
```

&ensp;&ensp;&ensp;&ensp;html部分由上，columns的title代表表头，key绑定接受后台传入字段名，数据部分绑定list，自动循环。
&ensp;&ensp;&ensp;&ensp;操作按钮需要手写，如其他字段一样；如数据无误，打印params会有相关行内数据，获取相关数据进行操作。

```
		data() {
			return{
				list: [],
				columns1: [
					{
                        title: '缺工岗位类别',
                        align:"center",
                        key: 'general',
                        children:
                            [
                              {
                                  title: '普工',
                                  key: 'lackWorker',
                                  align: 'center',

                              },
                              {
                                  title: '技术',
                                  key: 'lackTechnician',
                                  align: 'center',

                              },
                                {
                                  title: '管理人员',
                                  key: 'lackManager',
                                  align: 'center',
                                    width:110

                              },
                                {
                                  title: '其他',
                                  key: 'lackOther',
                                  align: 'center',

                              }
                            ]
                    },
                    {
				        title: '操作',
				        key: 'other2',
				        align: 'center',
				          render: (h, params) => {
				            return h("div", [
				              h(
				                "Button",
				                {
				                  props: {
				                    type: "primary",
				                    size: "small"
				                  },
				                  style: {
				                    marginRight: "5px",
				                    width:"24px",
				                    height:"24px",
				                    color:"#ffffff",
				                    backgroundColor:"#12BD42",
				                    backgroundRadius:"4px",
				                    borderRadius:"4px"
				                  },
				                  on: {
				                    click: () => {
				                      this.modal2 = true;
				                      let id = params.row.id
				                      api.qi_planEditor({id: id}).then((res) => {
				                        this.formValidate2= res.data;
				                      })
				                    }
				                  }
				                },
				              )
				            ]);
				          }
    				}
                ]
            }
        }
```
&ensp;&ensp;&ensp;&ensp;如需进行表格相关操作，如上 rander函数内进行。

以下翻页，其他data、接口等方面同vue类似。
```
 //页码
    changepage(value) {
        let datas = {
            pageNum: value,
            pageSize: this.pageSize,
        }
        api.qi_plan(datas).then(res=>{
            this.list = res.data.list;
        })
    },
    ```


