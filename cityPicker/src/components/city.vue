<template>
  <div id="app" v-cloak>
    <div class="container">
      <div class="div-search" :class="{'inpSelected' : inpSelected}">
        <input id="inp-search" class="inp-search" type="search" placeholder="请输入城市名称" @focus="inpSelected = true" v-model="term">
        <span class="searchCancel" @click="inpSelected = false , term = ''">取消</span>
      </div>
      <div class="main" :class="{'app' : isApp}">
        <div class="other-city" v-show="!inpSelected">
          <div class="currentlyCity">
            <h4 class="city-title city-title2" id="currentlyCity">当前定位城市</h4>
            <div class="city-panel">
              <div class="currently-city-val" @click="currentlyCity.name!='正在定位中' && setCityVal(currentlyCity.name,currentlyCity.id)">{{ currentlyCity.name }}</div>
            </div>
          </div>
          <div class="historyCity" v-show="historyCityList && historyCityList.length>0">
            <h4 class="city-title city-title2" id="historyCity">历史选择城市</h4>
            <div class="city-panel">
              <ul class="history-city">
                <li v-for="history_cityList in historyCityList" @click="setCityVal(history_cityList.hisCity,history_cityList.hisCityId)">{{ history_cityList.hisCity }}</li>
              </ul>
            </div>
          </div>
        </div>
        <div class="city-list">
          <div class="city-main" v-for="province_title , key , index in filterCityList" v-if="province_title.length>0">
            <h4 class="city-title" :class="{'selected' : showName == key}" @click="showName == key ? showName = '' : showName = key"
              :id="key">{{ key }}</h4>
            <ul v-show="showName == key">
              <li v-for="province_name in province_title" @click="setCityVal(province_name.name,province_name.id,province_name.pinyin)">{{ province_name.name }}</li>
            </ul>
          </div>
        </div>
        <ul class="right-city" v-show="!inpSelected">
          <li @click="showName = '当前' , showCity('currentlyCity')">当前</li>
          <li @click="showName = '历史' , showCity('historyCity')">历史</li>
          <li v-for="province_title , key in provinceList" @touchmove="anchorScroll(key,$event)" @touchend="anchorScroll1(key,$event)"
            @click="showName = key , showCity(key,$event)">{{ key }}</li>
        </ul>
      </div>
      <div class="filter-city" :class="{'dh-show' : dh}">{{ showName }}</div>
      <div class="no_result" v-show="!isResultNull">没有结果</div>

    </div>
  </div>
</template>

<script>
  export default {
    name: 'city',
    data() {
      return {
        showName: "A",
        dh: false,
        provinceList: {},
        inpSelected: false,
        term: "",
        historyCityList: [],
        currentlyCity: {
          name: "正在定位中",
          id: ""
        },
        isResultNull: true,
        isApp: false
      }
    },
    mounted() {
      var data = this.getStorageFC("_his_city");
      this.historyCityList = data || [];
      this.fetchData();
    },
    methods: {
      // 请求接口数据
      fetchData() {
        var self = this;
        var params = {

        }
        axios.get(
          '../../static/cityInfo.json'
        ).then((re) => {
          console.log(re.data);
          if (re.data.code == '1') {
            var data = re.data.data;
            var list = {};
            data.sort((a, b) => a.pinyin > b.pinyin ? 1 : -1);
            data.forEach((item, i) => {
              var firstPy = (item.pinyin || '').substring(0, 1);
              firstPy = (firstPy || '').toLocaleUpperCase();
              if (!list[firstPy]) {
                list[firstPy] = [];
              }
              list[firstPy].push(item);
            });
            self.provinceList = list;
            self.getLocal(data);
            // self.getLoaclBYBMAP(data, 40.05703033345938, 116.3084202915042);
          }

        }).catch(function (error) {});
      },

      setCityVal(name, id, pinyin) {
        console.log(name + '`' + id + '`' + pinyin);
        var cityObj = {};
        cityObj.hisCity = name;
        cityObj.hisCityId = id;
        this.historyCityList = this.historyCityList.filter(cityItem => cityItem.hisCity != cityObj.hisCity);
        this.historyCityList.unshift(cityObj);
        this.setStorageFC("_his_city", this.historyCityList);
        this.setStorageFC("_city", name);
        this.setStorageFC("_cityId", id);
      },
      setStorageFC(name, val) {
        return localStorage.setItem(name, JSON.stringify(val));
      },
      getStorageFC(name) {
        return localStorage.getItem(name) ? JSON.parse(localStorage.getItem(name)) : "";
      },
      // 获取定位信息 经纬度
      getLocal(data) {
        const self = this;
        navigator.geolocation.getCurrentPosition(function (res) {
          self.getLoaclBYBMAP(data, res.coords.latitude, res.coords.longitude);
        }, function (error) {
          switch (error.code) {
            case error.PERMISSION_DENIED:
              console.log("GPS:用户拒绝对获取地理位置的请求。");
              break;
            case error.POSITION_UNAVAILABLE:
              console.log("GPS:位置信息是不可用的。");
            case error.TIMEOUT:
              console.log("GPS:请求用户地理位置超时。");
              break;
            case error.UNKNOWN_ERROR:
              console.log("GPS:未知错误。");
              break;
          }
        }, {
          // 指定获取地理位置的超时时间，默认不限时，单位为毫秒
          timeout: 4000
        });
      },
      // 根据经纬度获取定位信息 城市---百度
      getLoaclBYBMAP(data, lat, lng) {
        const self = this;
        //http://api.map.baidu.com/geocoder/v2/?location=40.05703033345938,116.3084202915042&output=json&ak=I3H8uKPUFXUbnGMXkNQGS4Cr
        axios.get('http://api.map.baidu.com/geocoder/v2/?location=' + lat + ',' + lng +
          '&output=json&ak=I3H8uKPUFXUbnGMXkNQGS4Cr').then((res) => {
          var destName = String.prototype.slice.apply(res.data.result.addressComponent.city, [0, -1]);
          data.every((item, i) => {
            if (item.name == destName) {
              self.currentlyCity = {
                name: item.name,
                id: item.id
              };
              return false;
            }
            return true;
          })
        }).catch(function (err) {
          console.log(err);
        });
      },
      // 右侧字母切换
      showCity(id,e) {
        // console.log(e.currentTarget);
        document.querySelector('#' + id).scrollIntoView();
        this.dh = !this.dh;
        if (this.dh) {
          setTimeout(item => {
            this.dh = false;
          }, 350);
        }
      },
      anchorScroll: function (id,e) {
       console.log(e.currentTarget);
      },
      anchorScroll1: function (id,e) {
       console.log(e.currentTarget);
      },

    },
    computed: {
      filterCityList() {
        var _this = this;
        var term = this.term.slice(0, 1);
        var list = this.provinceList;
        var keys = Object.keys(list);
        var newList = {};
        var sum = 0;
        keys.forEach(keyItem => {
          newList[keyItem] = list[keyItem];
          if (term && list[keyItem].length > 0) {
            newList[keyItem] = list[keyItem].filter(rowItem =>
              rowItem.name.indexOf(term) >= 0 || rowItem.pinyin.toLocaleUpperCase().indexOf(term.toLocaleUpperCase()) ===
              0
            );
          }
        })
        for (var i in newList) {
          if (newList[i].length > 0) {
            sum++;
            if (term) {
              _this.showName = newList[i][0].pinyin.slice(0, 1).toLocaleUpperCase();
            }
            break;
          }
        }
        if (sum > 0) {
          _this.isResultNull = true;
        } else {
          _this.isResultNull = false;
        }
        return newList;
      }
    }
  }

</script>
<style scoped>
  [v-cloak] {
    display: none;
  }

  .container,
  .div-search {
    background-color: #f8f8f8;
    overflow: hidden;
  }

  .container .main {
    position: absolute;
    top: 57px;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: #f8f8f8;
    overflow-y: auto;
    -webkit-overflow-scrolling: touch;
  }

  .container .main.app {
    top: 56px;
  }

  .div-search {
    height: 53px;
    line-height: 50px;
    padding: 0 10px;
    margin-top: 4px;
    position: relative;
  }

  .searchCancel {
    color: #d30775;
    width: 13%;
    z-index: 2;
    position: absolute;
    right: 0;
    top: 0;
    height: 47px;
    text-align: center;
    -webkit-transform: translate3d(100%, 0, 0);
    -moz-transform: translate3d(100%, 0, 0);
    transform: translate3d(100%, 0, 0);
    -webkit-transition: -webkit-transform .3s ease;
    -moz-transition: -moz-transform .3s ease;
    transition: transform .3s ease;
  }

  .inp-search {
    width: 92%;
    height: 36px;
    margin-left: 20px;
    padding-left: 10px;
    padding-right: 10px;
    border: 1px solid #ddd;
    outline: none;
    border-radius: 3px;
    -webkit-tap-highlight-color: #ddd;
    -webkit-appearance: none;
    font-size: 14px;
    -webkit-transition: width .3s ease;
    -moz-transition: width .3s ease;
    transition: width .3s ease;
  }

  .div-search.inpSelected .inp-search {
    width: 83%;
  }

  .div-search.inpSelected .searchCancel {
    -webkit-transform: translate3d(0, 0, 0);
    -moz-transform: translate3d(0, 0, 0);
    transform: translate3d(0, 0, 0);
  }

  .city-title {
    font-weight: 400;
    font-size: 12px;
    padding: 10px 10px 8px;
    color: #6f6f6f;
    border-top: 1px solid #ddd;
  }

  .city-title.city-title2 {
    border-bottom: 1px solid #ddd;
  }

  .city-panel {
    padding: 10px 40px 10px 10px;
    background-color: #fff;
    overflow: hidden;
  }

  .currently-city-val {
    background: #f8f8f8;
    width: 30%;
    margin: 5px;
    text-align: center;
    -webkit-tap-highlight-color: transparent;
  }

  .city-panel .history-city {
    overflow: hidden;
    margin-bottom: 5px;
  }

  .city-panel .history-city li {
    float: left;
    width: 30%;
    border: 1px solid #ddd;
    height: 40px;
    line-height: 40px;
    text-align: center;
    margin-left: 5px;
    margin-top: 5px;
  }

  .city-list {}

  .city-list ul {
    padding-left: 10px;
    background-color: #fff;
    border-top: 1px solid #ddd;
  }

  .city-list ul li {
    height: 40px;
    line-height: 40px;
    border-bottom: 1px solid #eee;
  }

  .city-list ul li:nth-last-child(1) {
    border-bottom: 0;
  }

  .city-list h4 {
    position: relative;
  }

  .city-list h4:after {
    content: "";
    position: absolute;
    right: 53px;
    top: 5px;
    width: 7px;
    height: 7px;
    border-width: 2px;
    border-style: solid;
    border-color: transparent transparent rgb(170, 170, 170) rgb(170, 170, 170);
    margin: 5px -8px 0 0;
    -webkit-transform: rotate(-45deg);
    -moz-transform: rotate(-45deg);
    transform: rotate(-45deg);
  }

  .city-list h4.selected:after {
    top: 11px;
    border-color: rgb(170, 170, 170) transparent transparent rgb(170, 170, 170);
    -webkit-transform: rotate(45deg);
    -moz-transform: rotate(45deg);
    transform: rotate(45deg);
  }

  .right-city {
    position: fixed;
    right: 0;
    top: 100px;
    background: #fff;
    height: 100%;
    width: 40px;
    opacity: .7;
    text-align: center;
  }

  .right-city li {
    font-weight: 700;
    height: 3.5147%;
    color: #d5137b;
    font-size: 11px;
  }

  .filter-city {
    position: fixed;
    top: 50%;
    left: 50%;
    color: #fff;
    text-align: center;
    border-radius: 50%;
    -webkit-transform: translate(-50%, -50%);
    -moz-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
    width: 0;
    height: 0;
    overflow: hidden;
    font-size: 32px;
    background-color: rgba(211, 7, 117, .8);
  }

  .filter-city.dh-show {
    animation: dh .15s linear forwards;
    -webkit-animation: dh .15s linear forwards;
    -moz-animation: dh .15s linear forwards;
  }

  @keyframes dh {
    0% {
      transform: translate(-50%, -50%);
      opacity: 0;
    }
    100% {
      width: 84px;
      height: 84px;
      line-height: 84px;
      opacity: 1;
    }
  }

  @-webkit-keyframes dh {
    0% {
      -webkit-transform: translate(-50%, -50%);
      opacity: 0;
    }
    100% {
      width: 84px;
      height: 84px;
      line-height: 84px;
      opacity: 1;
    }
  }

  @-moz-keyframes dh {
    0% {
      -moz-transform: translate(-50%, -50%);
      opacity: 0;
    }
    100% {
      width: 84px;
      height: 84px;
      line-height: 84px;
      opacity: 1;
    }
  }

  .no_result {
    background-color: #fafafa;
    height: 100%;
    position: absolute;
    top: 120px;
    left: 0;
    right: 0;
    text-align: center;
    font-size: 16px;
    line-height: 40px;
  }

</style>
