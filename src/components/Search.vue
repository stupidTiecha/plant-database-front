<template>
    <div v-bind:id="searchContent">
        <b-form @submit="onSubmit" @reset="onReset" v-if="showContent.from" id="searchForm">
            <b-form-group  id="searchTypeGroup"
                          label="Search Advanced">
                <b-form-radio-group
                        v-model="searchForm.searchType"
                        :options="searchTypes"
                        value-field="id"
                        text-field="name"
                ></b-form-radio-group>
            </b-form-group>

            <b-form-group id="searchAllText"
                          v-show="searchForm.searchType === '1'">
                <label style="font-weight: bold ;font-size: large">Search All Text</label>
                <br/><label >Search Text :</label>
                <b-form-input v-model="searchForm.searchContent.textSearch.text"
                              placeholder="leave blank to return all records for a topic"></b-form-input>
            </b-form-group>
            <b-form-group id="searchByTopic"
                          v-show="searchForm.searchType === '2'">
                <label style="font-weight: bold ;font-size: large">Search By Topic</label>
                <br/><label >Search Text :</label>
                <b-form-input v-model="searchForm.searchContent.topicSearch.text"
                              placeholder="leave blank to return all records for a topic"></b-form-input>
                <label >Select topic :</label>
                <b-form-select v-model="searchForm.searchContent.topicSearch.topic"
                               :options="searchTopics" ></b-form-select>
            </b-form-group>

            <b-form-group id="searchByField"
                          v-show="searchForm.searchType === '3'">
                <label style="font-weight: bold ;font-size: large">Search By Field</label>
                <b-container fluid>
                    <b-row >
                        <b-col sm="4">
                            <label >Field 1 :</label>
                            <b-form-select v-model="searchForm.searchContent.searchByField.field1"
                                           :options="searchFields" ></b-form-select>
                        </b-col>
                        <b-col sm="8">
                            <label >Search Text 1 :</label>
                            <b-form-input v-model="searchForm.searchContent.searchByField.text1"
                                          placeholder="search words here"></b-form-input>
                        </b-col>
                    </b-row>
                    <b-row >
                        <b-col sm="4">
                            <label >Field 2 :</label>
                            <b-form-select v-model="searchForm.searchContent.searchByField.field2"
                                           :options="searchFields" ></b-form-select>
                        </b-col>
                        <b-col sm="8">
                            <label >Search Text 2 :</label>
                            <b-form-input v-model="searchForm.searchContent.searchByField.text2"
                                          placeholder="ignored if Search Text 1 blank"></b-form-input>
                        </b-col>
                    </b-row>
                    <b-row >
                        <b-col sm="4">
                            <label >Field 3 :</label>
                            <b-form-select v-model="searchForm.searchContent.searchByField.field3"
                                           :options="searchFields" ></b-form-select>
                        </b-col>
                        <b-col sm="8">
                            <label >Search Text 3 :</label>
                            <b-form-input v-model="searchForm.searchContent.searchByField.text3"
                                          placeholder="ignored if Search Text 1 blank"></b-form-input>
                        </b-col>
                    </b-row>
                </b-container>
            </b-form-group>
            <b-form-group id="searchScopeGroup"
                          label="Search Scope">
                <b-form-radio-group
                        v-model="searchForm.searchScope"
                        :options="searchScopes"
                        value-field="id"
                        text-field="name"
                ></b-form-radio-group>
            </b-form-group>
            <b-form-group>
                <b-button type="submit" variant="primary">Submit</b-button>
                <b-button type="reset" variant="danger">Reset</b-button>
            </b-form-group>
        </b-form>
    </div>
</template>

<script>

    export default {
        name: "Search",
        data() {
            return {
                searchContent : 'searchContent',
                searchForm : {
                    searchType : '1',
                    searchScope : '1',
                    searchContent : {
                        topicSearch : {
                            text : '',
                            topic : '01'
                        },
                        textSearch : {
                            text : '',
                        },
                        searchByField : {
                            field1 : '',
                            text1 : '',
                            field2 : '1',
                            text2 : '',
                            field3 : '1',
                            text3 : '',
                        }
                    },
                },
                showContent : {
                    from : true,
                },
                searchTypes : [
                    {id : '1',name : 'search all text'} ,
                    {id : '2',name : 'search by topic'} ,
                    {id : '3',name : 'search by fields'}
                ],
                searchScopes : [
                    {id : '1',name : 'search whole database'},
                    {id : '2',name : 'search plants only'},
                    {id : '3',name : 'search references only'}

                ],
                searchTopics : [
                    {value : '01' ,text : 'cabbage tree (ti or Cordyline) records'},
                    {value : '02' ,text : 'dyes'},
                    {value : '03' ,text : 'ethnobotany theory and methods'},
                    {value : '04' ,text : 'fernroot (Pteridium or bracken)'},
                    {value : '05' ,text : 'ferns and fern allies'},
                    {value : '06' ,text : 'flax (Phormium) all records'},
                    {value : '07' ,text : 'flax (Phormium) cultivars (not Orchiston)'},
                    {value : '08' ,text : 'flax (Phormium) cultivation'},
                    {value : '09' ,text : 'flax (Phormium) fibre records'},
                    {value : '10' ,text : 'flax (Phormium) industry'},
                    {value : '11' ,text : 'flax (Phormium) insects and diseases'},
                    {value : '12' ,text : 'flax (Phormium) Orchiston cultivars'},
                    {value : '13' ,text : 'flax (Phormium) references'},
                    {value : '14' ,text : 'flax (Phormium) weaving/plaiting'},
                    {value : '15' ,text : 'flax (Phormium) yellow-leaf disease'},
                    {value : '16' ,text : 'food plants - other'},
                    {value : '17' ,text : 'food plants- references'},
                    {value : '18' ,text : 'fungi'},
                    {value : '19' ,text : 'key references '},
                    {value : '20' ,text : 'kūmara cultivars'},
                    {value : '21' ,text : 'kūmara references'},
                    {value : '22' ,text : 'medicinal plants'},
                    {value : '23' ,text : 'medicinal references '},
                    {value : '24' ,text : 'mosses, liverworts, lichen'},
                    {value : '25' ,text : 'nomenclature'},
                    {value : '26' ,text : 'Pandanus'},
                    {value : '27' ,text : 'potatoes'},
                    {value : '28' ,text : 'scented plants'},
                    {value : '29' ,text : 'seaweeds'},
                    {value : '30' ,text : 'traditional knowledge/rights'},
                    {value : '31' ,text : 'weaving/fibre plants - other than flax'}
                ],
                searchFields : [
                    {value : "1" , text : "(none)"},
                    {value : "2" , text : "Author"},
                    {value : "3" , text : "Date"},
                    {value : "4" , text : "Editor"},
                    {value : "5" , text : "Publisher"},
                    {value : "6" , text : "Title"},
                    {value : "7" , text : "---------------------------------"},
                    {value : "8" , text : "Botanical Name"},
                    {value : "9" , text : "Common Name"},
                    {value : "10" , text : "Family Name"},
                    {value : "11" , text : "Maori Name"},
                    {value : "12" , text : "---------------------------------"},
                    {value : "13" , text : "Chemistry"},
                    {value : "14" , text : "Construction"},
                    {value : "15" , text : "Description"},
                    {value : "16" , text : "Domestic"},
                    {value : "17" , text : "Dyes"},
                    {value : "18" , text : "Environment"},
                    {value : "19" , text : "Fibre"},
                    {value : "20" , text : "Fishing and Hunting"},
                    {value : "21" , text : "Food"},
                    {value : "22" , text : "Links"},
                    {value : "23" , text : "Medicinal"},
                    {value : "24" , text : "Note"},
                    {value : "25" , text : "Pastime"},
                    {value : "26" , text : "Proverbs"},
                    {value : "27" , text : "Scent"},
                    {value : "28" , text : "Toxins"},
                    {value : "29" , text : "Traditions"},
                    {value : "30" , text : "---------------------------------"},
                    {value : "31" , text : "Link words"},

                ],
            }
        },
        methods : {
            onSubmit : function (evt) {
                evt.preventDefault();

                //将提交表单数据复制一份用以精简，不然会影响页面数据
                // console.log(JSON.stringify(this.searchForm));
                let submitForm = JSON.parse(JSON.stringify(this.searchForm));
                if (submitForm.searchType === '1') {
                    submitForm.searchContent = submitForm.searchContent.textSearch;
                } else if (submitForm.searchType === '2') {
                    submitForm.searchContent = submitForm.searchContent.topicSearch;
                } else {
                    submitForm.searchContent = submitForm.searchContent.searchByField;
                }
                console.log(JSON.stringify(submitForm));
                //提交数据到后台
                // this.$http.post('https://www.baidu.com/baidu?&ie=utf-8&word=bootstrapVuee',submitForm).then(response => {
                //     //查询结果获取
                //     console.log(response);
                // },response => {
                //     //后台响应失败
                //     console.log(response);
                // })
            },
            onReset : function (evt) {
                evt.preventDefault();
                this.searchForm = JSON.parse('{"searchType":"1","searchScope":"1","searchContent":{"topicSearch":{"text":"","topic":"01"},"textSearch":{"text":""},"searchByField":{"field1":"","text1":"","field2":"","text2":"","field3":"","text3":""}}}');
            },
        }
    }
</script>


<style scoped>
#searchForm {
    left: 60px;
    top: 40px;
    width: 70% ;
    min-height: 520px;
    max-width: 700px;
    text-align: left;
    position: relative;
}
#searchContent {
    height: 100%;
    width: inherit;
    position: relative;
    background-color: whitesmoke;
}
</style>