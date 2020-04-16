<template>
  <v-app id="inspire">
    
    <v-app-bar
      app
      clipped-left
      dark
      class="indigo"
    >
      <v-app-bar-nav-icon />
      <v-toolbar-title v-bind:style="{ fontFamily: fontFamily, fontSize: fontSize + 'px' }">数栈</v-toolbar-title>
    </v-app-bar>

    <v-content>
       <v-container fluid grid-list-md fill-height>
        <v-layout row wrap>
          <v-flex xs12 sm3 md3>
            <v-card color="white" height="100%">
              <v-img
                src="https://cdn.vuetifyjs.com/images/cards/desert.jpg"
                 class="white--text align-end"
                aspect-ratio="2.75"
              >
                <v-card-title >看板参数</v-card-title>
              </v-img>
              
                            
              <div>
                <v-expansion-panels class="mb-10" multiple>
                  <v-expansion-panel>
                    <v-expansion-panel-header expand-icon="mdi-go-kart" >系统&nbsp;{{activeBtn}}</v-expansion-panel-header>
                    <v-expansion-panel-content>
                      <div class="overflow-hidden">    
                        <v-bottom-navigation
                          v-model="activeBtn"
                          hide-on-scroll
                          color="indigo"
                        >
                        <v-container
                          id="scroll-target"
                          style="max-height: 400px"
                          class="overflow-y-auto"
                        >
                          <v-row wrap>
                          <v-btn value="燃油">
                            <span>燃油</span>                            
                          </v-btn>

                          <v-btn value="中冷">
                            <span>中冷</span>                            
                          </v-btn>

                          <v-btn value="涡轮">
                            <span>涡轮</span>                            
                          </v-btn>
                          <v-btn value="EMS">
                            <span>EMS</span>                            
                          </v-btn>

                          <v-btn value="进气系统">
                            <span>进气系统</span>                            
                          </v-btn>

                          <v-btn value="冷却系统">
                            <span>冷却系统</span>                            
                          </v-btn>
                          <v-btn value="发动机">
                            <span>发动机</span>                            
                          </v-btn>
                          <v-btn value="变速器">
                            <span>变速器</span>                            
                          </v-btn>
                          </v-row>
                        </v-container>
                        </v-bottom-navigation>                    
                      </div>
                    </v-expansion-panel-content>
                  </v-expansion-panel>
                  <v-expansion-panel>
                    <v-expansion-panel-header expand-icon="mdi-ubuntu" >项目&nbsp;{{dialogm1}}</v-expansion-panel-header>
                    <v-expansion-panel-content>
                      <h6 @click="openDialog()">请选择一个项目......</h6>
                    </v-expansion-panel-content>
                      <v-row>
                        <v-dialog v-model="dialog" scrollable max-width="200px" v-if="dialog">                          
                          <v-card>
                            <v-card-title><h6>选择一个项目</h6></v-card-title>
                            <v-divider></v-divider>
                            <v-card-text>
                              <v-radio-group v-model="dialogm1" column>
                                <v-radio label="A12" value="A12" ></v-radio>
                                <v-radio label="GS10" value="GS10"></v-radio>
                                <v-radio label="GA8" value="GA8"></v-radio>        
                              </v-radio-group>
                            </v-card-text>
                            <v-divider></v-divider>
                            <v-card-actions>
                              <v-btn color="blue darken-1" text @click="dialog = false">关闭</v-btn>
                              
                            </v-card-actions>
                          </v-card>
                        </v-dialog>
                      </v-row>
                  </v-expansion-panel>
                  <v-expansion-panel>
                    <v-expansion-panel-header expand-icon="mdi-map" >阶段&ensp;{{selectStage}}</v-expansion-panel-header>
                    <v-expansion-panel-content>
                      <v-select v-model="selectStage"
                        :items="items"
                        :menu-props="{ top: true, offsetY: true }"
                        dense
                  
                      ></v-select>
                    </v-expansion-panel-content>
                  </v-expansion-panel>
                  <v-expansion-panel>
                    <v-expansion-panel-header expand-icon="mdi-turnstile" >布点</v-expansion-panel-header>
                    <v-expansion-panel-content>
                      <v-select 
                        v-model="selectPosition"
                        :items="position[activeBtn]"
                        attach
                        chips
                        multiple
                      ></v-select>
                    </v-expansion-panel-content>
                  </v-expansion-panel>
                  <v-expansion-panel>
                    <v-expansion-panel-header expand-icon="mdi-engine" >工况</v-expansion-panel-header>
                    <v-expansion-panel-content>
                      <div v-if="selectPosition.length > 1">
                        <v-select 
                          v-model="selectLoad"
                          :items="load"
                          multiple
                          attach
                          chips                  
                        ></v-select>
                      </div>
                      <div v-else>
                        <v-select 
                          v-model="selectLoad"
                          :items="load"
                          attach
                          multiple
                          chips                  
                        ></v-select>
                      </div>
                      
                    </v-expansion-panel-content>
                  </v-expansion-panel>
                </v-expansion-panels>
                
                
              </div>         
            </v-card>

          </v-flex>
          <v-flex xs12 sm9 md9>
            <v-card dark tile flat color="pink darken-4" height="100%">
              <v-card-text>#2</v-card-text>
            </v-card>
          </v-flex>
        </v-layout> 
      </v-container>
    </v-content>

    <v-footer app dark
      class="indigo">
      <span>2020</span>
    </v-footer>
  </v-app>
</template>

<script>
  export default {
    props: {
      source: String,
    },
    data: () => ({
      position:{'燃油':['燃油箱表面','油管（催化器侧）表面'],'进气系统':['进气管进气温度（进口温度）','空滤器表面'],'EMS':['ECU环境']},
      load:['城市平路','城市变速','高超平路','高超怠速','高爬平路','高爬怠速','高爬爬坡','高爬熄火'],
      selectPosition:[],
      selectLoad:[],
      items:['ET1', 'ET2', 'Alpha', 'Beta'],
      selectStage:'',
      dialogm1:'',
      dialog:false,
      activeBtn:'',
      fontFamily: '',
      fontSize: 28,
      drawer: null,
      lorem: `Lorem ipsum dolor sit amet, mel at clita quando. Te sit oratio vituperatoribus, nam ad ipsum posidonium mediocritatem, explicari dissentiunt cu mea. Repudiare disputationi vim in, mollis iriure nec cu, alienum argumentum ius ad. Pri eu justo aeque torquatos.`
    }),
    created () {
      this.$vuetify.theme.dark = false
    },
    methods:{
      openDialog: function() {
			console.log('open Dialog');
			this.dialog = true;
		}
    }
  }
</script>