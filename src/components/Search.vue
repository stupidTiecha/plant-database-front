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
                          v-show="searchForm.searchType === '0'">
                <label style="font-weight: bold ;font-size: large">Search All Text</label>
                <br/><label >Search Text :</label>
                <b-form-input v-model="searchForm.searchContent.textSearch.text"
                              placeholder="leave blank to return all records"></b-form-input>
            </b-form-group>
            <b-form-group id="searchByTopic"
                          v-show="searchForm.searchType === '1'">
                <label style="font-weight: bold ;font-size: large">Search By Topic</label>
                <br/><label >Search Text :</label>
                <b-form-input v-model="searchForm.searchContent.topicSearch.text"
                              placeholder="leave blank to return all records for a topic"></b-form-input>
                <label >Select topic :</label>
                <b-form-select v-model="searchForm.searchContent.topicSearch.topic"
                               :options="searchTopics" ></b-form-select>
            </b-form-group>

            <b-form-group id="searchByField"
                          v-show="searchForm.searchType === '2'">
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
        <div id="searchResult" v-if="showContent.result">
            <b-button  v-on:click="backStep(1)" >&lt;- Back To Search </b-button>
            <div class="data" v-if="showContent.noResult">
                <a><span>No Result Found </span></a>
            </div>
            <br/><span style="font-weight: bold;font-size: larger">total records : {{result.totalRecords}}</span>
            <div  class="data" v-for="(item,index) in result.resultList" :key="item.item_id">
                <a href="#" v-on:click="getItem($event)" :value="item.item_id" :id="index"><span>{{item.item_title}}</span></a>
            </div>
            <div class="overflow-auto">
                <b-pagination
                        v-model="result.page"
                        :total-rows="result.totalRecords"
                        :per-page="10"
                        v-on:change="goToPage"
                        first-number
                        last-number
                        base-url="#">
                </b-pagination>
            </div>

        </div>
        <div id="searchDetail" v-if="showContent.detail">
            <div v-if="detail !== null" style="height: 80px;line-height: 70px;font-weight: bold;font-size: 30px">
                <span v-if="detail[0].itemType !== 'Reference'">Plant Use Detail</span>
                <span v-else>Reference Detail</span>
            </div>
            <b-button  v-on:click="backStep(2)" >&lt;- Back To SearchResult </b-button>
            <div class="data" v-if="showContent.noDetail">
                <a><span>No Detail Found </span></a>
            </div>
            <div  class="data" v-for="(item,index) in detail" :key="index" >
                <div v-if="item.noteClass === 'Image'">

                    <b-row >
                        <b-col sm="1">

                        </b-col>
                        <b-col sm="9">
                            <span>Click to view larger image.</span><br/>
                            <b-img v-for="(pic,index) in item.noteImages.images" :key="index"
                                   :src="'http://data.landcareresearch.co.nz/' + pic + '/ThumbNail'"
                                   v-on:click="getLargePic(pic,item.noteImages.caption)"
                                   :alt="item.noteImages.caption"></b-img>
                        </b-col>
                    </b-row>

                </div>
                <div v-else-if="item.noteClass !== 'Image'">
                    <b-row >
                        <b-col sm="2">
                            <span style="font-weight: bold">{{item.title}} : </span>
                        </b-col>
                        <b-col sm="9">
                            <div  v-html="item.content" class="item-content"></div>
                        </b-col>
                    </b-row>
                </div>
            </div>

        </div>
        <b-modal :title="imageModal.title" id="image-modal" hide-footer size="lg">
            <b-img  :src="imageModal.url" :alt="imageModal.title" style="width: 770px"></b-img>
            <b-button class="mt-3" block @click="$bvModal.hide('image-modal')">Close</b-button>
        </b-modal>
    </div>
</template>

<script>

    import Config from '../assets/js/Config.js';


    export default {
        name: "Search",
        data() {
            return {
                searchContent : 'searchContent',
                searchValue : "",
                searchForm : {
                    searchType : '0',
                    searchScope : '1',
                    searchContent : {
                        topicSearch : {
                            text : '',
                            topic : 'flax (Phormium) weaving/plaiting',
                            page: 0
                        },
                        textSearch : {
                            text : '',
                            page: 0
                        },
                        searchByField : {
                            field1 : '',
                            text1 : '',
                            field2 : 'none',
                            text2 : '',
                            field3 : 'none',
                            text3 : '',
                            page: 0
                        }
                    },
                },
                showContent : {
                    from : true,
                    result : false,
                    noResult : false,
                    detail : false,
                    noDetail : false,
                },
                searchTypes : [
                    {id : '0',name : 'search all text'} ,
                    {id : '1',name : 'search by topic'} ,
                    {id : '2',name : 'search by fields'}
                ],
                searchScopes : [
                    {id : '1',name : 'search whole database'},
                    {id : '2',name : 'search plants only'},
                    {id : '3',name : 'search references only'}

                ],
                searchTopics : [
                    {value : 'flax (Phormium) weaving/plaiting',text:'flax (Phormium) weaving/plaiting'},
                    {value : 'flax (Phormium) yellow-leaf disease',text:'flax (Phormium) yellow-leaf disease'},
                    {value : 'flax (Phormium) all records',text:'flax (Phormium) all records'},
                    {value : 'fernroot (Pteridium or bracken)',text:'fernroot (Pteridium or bracken)'},
                    {value : 'food plants - other',text:'food plants - other'},
                    {value : 'flax (Phormium) fibre records',text:'flax (Phormium) fibre records'},
                    {value : 'flax (Phormium) industry',text:'flax (Phormium) industry'},
                    {value : 'flax (Phormium) Orchiston cultivars',text:'flax (Phormium) Orchiston cultivars'},
                    {value : 'flax (Phormium) cultivars (not Orchiston)',text:'flax (Phormium) cultivars (not Orchiston)'},
                    {value : 'flax (Phormium) insects and diseases',text:'flax (Phormium) insects and diseases'},
                    {value : 'flax (Phormium) references',text:'flax (Phormium) references'},
                    {value : 'flax (Phormium) cultivation',text:'flax (Phormium) cultivation'},
                    {value : 'dyes',text:'dyes'},
                    {value : 'medicinal plants',text:'medicinal plants'}
                ],
                searchFields : [
                    {value :'none', text :'(none)'},
                    {value :'Author, secondary', text :'Author, secondary'},
                    {value :'Book Author', text :'Book Author'},
                    {value :'Author, primary', text :'Author, primary'},
                    {value :'Orthographic Variances', text :'Orthographic Variances'},
                    {value :'Environment', text :'Environment'},
                    {value :'Medicinal', text :'Medicinal'},
                    {value :'Note', text :'Note'},
                    {value :'Maori Names', text :'Maori Names'},
                    {value :'Common Names', text :'Common Names'},
                    {value :'Botanical Name', text :'Botanical Name'},
                    {value :'Previous names', text :'Previous names'},
                    {value :'Description', text :'Description'},
                    {value :'Food', text :'Food'},
                    {value :'Proverbs', text :'Proverbs'},
                    {value :'Toxins', text :'Toxins'},
                    {value :'Construction', text :'Construction'},
                    {value :'Fishing and Hunting', text :'Fishing and Hunting'},
                    {value :'Chemistry', text :'Chemistry'},
                    {value :'Links', text :'Links'},
                    {value :'Scent', text :'Scent'},
                    {value :'Traditions', text :'Traditions'},
                    {value :'Name Images', text :'Name Images'},
                    {value :'Fibre', text :'Fibre'},
                    {value :'Dyes', text :'Dyes'},
                    {value :'Pastime', text :'Pastime'},
                    {value :'Domestic', text :'Domestic'},
                ],
                result : {
                    totalRecords: 0,
                    page : 1,
                    resultList : [],
                },
                detail : [
                    {
                        noteFormId : "",
                        content : "",
                        title : "",
                        noteClass :"",
                        itemType :"",
                        displayOrder:0,
                        noteImages : {
                            images : [],
                            caption : ""
                        }
                    }
                ],
                imageModal : {
                    title : "",
                    url : ""
                },

            }
        },
        methods : {
            onSubmit : function (evt) {
                evt.preventDefault();

                //Make a copy of the submitted form data for streamlining, otherwise it will affect the page data
                // console.log(JSON.stringify(this.searchForm));
                let submitForm = JSON.parse(JSON.stringify(this.searchForm));
                if (submitForm.searchType === '0') {
                    this.searchValue = submitForm.searchContent.textSearch.text;
                    submitForm.searchContent = submitForm.searchContent.textSearch;
                } else if (submitForm.searchType === '1') {
                    this.searchValue = submitForm.searchContent.topicSearch.text;
                    submitForm.searchContent = submitForm.searchContent.topicSearch;
                } else {
                    let text1 = submitForm.searchContent.searchByField.text1;
                    let text2 = submitForm.searchContent.searchByField.text2;
                    let text3 = submitForm.searchContent.searchByField.text3;
                    if (null === text1) {
                        this.searchValue = "";
                    } else {
                        this.searchValue = text1 + " " + text2 + " " + text3;
                    }
                    submitForm.searchContent = submitForm.searchContent.searchByField;
                }
                // console.log(JSON.stringify(submitForm));
                this.searchPost(submitForm);
            },
            onReset : function (evt) {
                evt.preventDefault();
                this.searchForm = JSON.parse('{"searchType":"0","searchScope":"1","searchContent":{"topicSearch":{"text":"","topic":"01","page":0},"textSearch":{"text":"","page":0},"searchByField":{"field1":"","text1":"","field2":"","text2":"","field3":"","text3":"","page":0}}}');
            },
            getItem : function (e) {
                let value = e.currentTarget;
                let str = value.getAttribute("value");
                this.$http.get(Config.baseUrl() + 'plant-data/api/plant?itemId=' + str).then(response => {
                    let detail = response.data.detail;
                    this.showContent.noDetail = (detail === null);
                    this.detail = detail;
                    //Hide and show forms
                    this.backStep(3);
                },response => {
                    //failed
                    console.log(response);
                })
                // console.log(str);
            },
            backStep : function (num) {
                //Hide and show search form
                this.showContent.from = (1 === num);
                this.showContent.result = (2 === num);
                this.showContent.detail = (3 === num);
            },
            goToPage : function (page) {
                let submitForm = JSON.parse(JSON.stringify(this.searchForm));
                if (submitForm.searchType === '0') {
                    submitForm.searchContent = submitForm.searchContent.textSearch;
                } else if (submitForm.searchType === '1') {
                    submitForm.searchContent = submitForm.searchContent.topicSearch;
                } else {
                    submitForm.searchContent = submitForm.searchContent.searchByField;
                }
                submitForm.searchContent.page = page - 1;
                this.searchPost(submitForm);
                // console.log(page);
            },
            searchPost : function (submitForm) {
                // Submit data to the backend
                this.$http.post(Config.baseUrl() + 'plant-data/api/search',submitForm).then(response => {
                    //Get query results
                    let result = response.data.detail.result;
                    let records = response.data.detail.totalRecords;
                    let pageNum = response.data.detail.page;
                    // console.log(records);
                    //Hide and show forms
                    this.backStep(2);
                    //Assignment binding
                    this.result.totalRecords = records;
                    this.result.page = pageNum;
                    this.result.resultList = result;
                    this.showContent.noResult = (result.length === 0);
                    // console.log(result);
                },response => {
                    //failed
                    console.log(response);
                })
            },
            hasValue : function (list,str) {
                list.forEach(value => {
                    if (str.indexOf(value) === -1) {
                        return false;
                    }
                });
                return true;
            },
            setLink : function () {
                let elements = this.$el.getElementsByClassName("item-content");

                for (let i = 0; i < elements.length; i++) {
                    let regexList = [];
                    let replaceMap = new Map();
                    if ( this.searchValue !== ''){
                        let strings = this.searchValue.split(' ');
                        // console.log(strings);
                        let count = 0;
                        let reg = new RegExp('(AND|OR|NOT|[&|+|-|\*|%])');
                        strings.forEach(value => {
                            let s = value.trim();
                            if (s !== '' && !reg.test(s)) {
                                let regExp = new RegExp(value,'gi');
                                regexList[count++] = regExp;
                                replaceMap.set(regExp,'<span style="background-color:greenyellow">' + value + '</span>');
                            }
                        });
                    }
                    let element = elements[i];
                    let elA = element.getElementsByTagName("a");
                    let elText = element.getElementsByTagName("text");
                    let elValue = element.getElementsByTagName("value");
                    let elP = element.getElementsByTagName("p");
                    if (elA.length > 0){
                        for (let j = 0; j < elA.length; j++) {
                            let aTag = elA[j];
                            aTag.setAttribute("href","#");
                            let id = aTag.getAttribute("citeditemid");
                            aTag.setAttribute("value",id);
                            aTag.addEventListener("click",this.getItem);
                            if (aTag.innerHTML !== null && regexList.length > 0) {
                                regexList.forEach(value => aTag.innerHTML = aTag.innerText.replace(value,replaceMap.get(value)));
                            }
                        }
                    }
                    // console.log(regexList);
                    // console.log(replaceMap);
                    if (regexList.length > 0 !== null ) {
                        if (elText.length > 0){
                            for (let j = 0; j < elText.length; j++) {
                                let textTag = elText[j];
                                let nodeValue = textTag.childNodes[0].nodeValue;
                                if (nodeValue !== null && this.hasValue(regexList,nodeValue)) {
                                    // console.log(elText);
                                    regexList.forEach(value =>  nodeValue = nodeValue.replace(value,replaceMap.get(value)));
                                    // console.log(nodeValue);
                                    let temp = document.createElement("span");
                                    temp.innerHTML = nodeValue;
                                    textTag.childNodes[0].replaceWith(temp);
                                }
                            }
                        }
                        if (elValue.length > 0){
                            for (let j = 0; j < elValue.length; j++) {
                                let valueTag = elValue[j];
                                let nodeValue = valueTag.childNodes[0].nodeValue;
                                if (nodeValue !== null && this.hasValue(regexList,nodeValue)) {
                                    // console.log(elValue);
                                    regexList.forEach(value =>  nodeValue = nodeValue.replace(value,replaceMap.get(value)));
                                    // console.log(nodeValue);
                                    let temp = document.createElement("span");
                                    temp.innerHTML = nodeValue;
                                    valueTag.childNodes[0].replaceWith(temp);
                                }
                            }
                        }
                        if (elP.length > 0){
                            for (let j = 0; j < elP.length; j++) {
                                let pTag = elP[j];
                                let nodeValue = pTag.childNodes[0].nodeValue;
                                if (nodeValue !== null &&  this.hasValue(regexList,nodeValue)) {
                                    // console.log(elP);
                                    regexList.forEach(value =>  nodeValue = nodeValue.replace(value,replaceMap.get(value)));
                                    // console.log(nodeValue);
                                    let temp = document.createElement("span");
                                    temp.innerHTML = nodeValue;
                                    pTag.childNodes[0].replaceWith(temp);
                                }
                            }
                        }
                    }

                }


            },
            getLargePic :function (pic,title) {
                this.imageModal.title = title;
                this.imageModal.url = 'http://data.landcareresearch.co.nz/' + pic;
                this.$bvModal.show('image-modal');
            }

        },
        updated() {
            if (this.showContent.detail) {
                //Set keyword highlighting and links
                this.setLink();
            }
        }
    }
</script>


<style scoped>
#searchForm {
    left: 200px;
    top: 40px;
    width: 70% ;
    min-height: 520px;
    max-width: 700px;
    text-align: left;
    position: relative;
    color: black;
}
#searchContent {
    height: 100%;
    width: inherit;
    position: relative;
    background-color: white ;
}

#searchResult {
    left: 200px;
    top: 40px;
    width: 85% ;
    min-height: 520px;
    max-width: 1100px;
    text-align: left;
    position: relative;
    color: black;
}
#searchResult .data {
    height: 35px;
    width: inherit;
    background-color: snow;
    border-radius: 4px;
    border-style: solid;
    border-color: white;
    text-shadow: #AAAAAA;
}
#searchResult .data:hover{
    background-color: whitesmoke;
}
#searchResult .data a {
    display: inline-block;
    width: 100%;
    line-height: inherit;
}

#searchResult .data a  span {
    width: inherit;
    font-size: 22px;
    display: inline-block;
    line-height: inherit;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
}

#searchDetail {
    left: 200px;
    top: 40px;
    text-align: left;
    width: 85%;
    position: relative;
}

#searchDetail .data {
    height: auto;
    width: inherit;
    background-color: snow;
    border-radius: 4px;
    border-style: solid;
    border-color: white;
    text-shadow: #AAAAAA;
}
#searchDetail .data:hover{
    background-color: whitesmoke;
}



</style>