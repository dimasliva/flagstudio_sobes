<template>
  <div class="home">
      <div class="side">
          <div class="side_country">
              <div @click="init(this.countries.geo_ru); activeBtn('rus');" :class="isActiveRu ? 'side_country_btn active' : 'side_country_btn'">Россия</div>
              <div @click="init(this.countries.geo_be); activeBtn('bel');" :class="isActiveBu ? 'side_country_btn active' : 'side_country_btn'">Белоруссия</div>
          </div>
              <ul class="side_office" v-for="(city, i) in selectedCountry">
                  <li>
                      <div :class="city.hideList ? 'side_office_city active' : 'side_office_city'" @click="openList(selectedCountry, city, i)">
                          <span>{{city.city}}</span>
                          <font-awesome-icon icon="angle-up" :class="city.hideList ? 'side_office_city_icon active' : 'side_office_city_icon'" />
                      </div>
                      <ul class="side_office_list" v-for="office in city.offices">
                          <li class="side_office_header">{{office.title}}</li>
                          <li class="side_office_fio">{{office.fio}}</li>
                          <li class="side_office_numbers">
                          <span class="side_office_num" v-for="number in office.numbers">
                              {{number}}
                          </span>
                          </li>
                          <li class="side_office_email">{{office.email}}</li>
                      </ul>
                  </li>
              </ul>
      </div>
    <div id="map" class="map" style="width: 100vw; height: 100vh"></div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios'
import circle from '/src/assets/circle.png'
export default {
  name: 'HomeView',
  data: () => ({
      selectedCountry: [],
      countries:
          {
              russia: [
                  {
                      city: 'Брянск',
                      hideList: false,
                      offices: [
                          {
                              title: 'Офис Брянск',
                              fio: 'Белкин Рома Евгеньев',
                              numbers: [
                                  '+7999-222-22-22', '+7(999)-222-22-22'
                              ],
                              email: 'pupalupe@flagstudio.ru',
                          },
                          {
                              title: 'Офис Брянск-Полимер',
                              fio: 'Полимерович Сергей Александрович',
                              numbers: [
                                  '+7999-222-22-22', '+7(123)-222-22-22'
                              ],
                              email: 'pipapypa@flagstudio.ru',
                          }
                      ],
                      geo_center: {
                          latitude: 53.25008960136068,
                          longitude: 34.35611189404297,
                          balloonX: 53.24447526078375,
                          balloonY: 34.36597424339776,
                          balloonContent: [
                              '<div class="ballon">',
                              '<div class="ballon_office">Офис Брянск</div>',
                              '<div>Белкин Рома Евгеньев</div>',
                              '<div class="ballon_numbers">',
                              '<span style="margin-right: 10px;">+7999-222-22-22</span>',
                              '<span>+7(999)-222-22-22</span>',
                              '</div>',
                              '<div class="ballon_email">pupalupe@flagstudio.ru</div>',
                              '</div>'
                          ]
                      },

                  },
                  {
                      city: 'Калуга',
                      hideList: false,
                      offices: [
                          {
                              title: 'Офис Банк России',
                              fio: 'Банкирович Олег Тинькофф',
                              numbers: [
                                  '+7999-222-22-22', '+7(999)-222-22-22'
                              ],
                              email: 'username@flagstudio.ru'
                          },
                          {
                              title: 'Офис Красивые брови',
                              fio: 'Красавчиков Дмитрий Олегович',
                              numbers: [
                                  '+7999-222-22-22', '+7(123)-222-22-22'
                              ],
                              email: 'brovki@flagstudio.ru'
                          }
                      ],
                      geo_center: {
                          latitude: 54.513702671782774,
                          longitude: 36.262755737058036,
                          balloonX: 54.51955999279444,
                          balloonY: 36.26577688477283,
                          balloonContent: [
                              '<div class="ballon">',
                              '<div class="ballon_office">Офис Красивые брови</div>',
                              '<div>Красавчиков Дмитрий Олегович</div>',
                              '<div class="ballon_numbers">',
                              '<span style="margin-right: 10px;">+7999-222-22-22</span>',
                              '<span>+7(999)-222-22-22</span>',
                              '</div>',
                              '<div class="ballon_email">brovki@flagstudio.ru</div>',
                              '</div>'
                          ]
                      }
                  },
                  {
                      city: 'Химки',
                      hideList: false,
                      offices: [
                          {
                              title: 'Альфа Страхование',
                              fio: 'Страховой Петя Владимирович',
                              numbers: [
                                  '+7345-222-22-22', '+7(543)-222-22-22'
                              ],
                              email: 'brovki@flagstudio.ru'
                          }
                      ],
                      geo_center: {
                          latitude: 55.88776502507303,
                          longitude: 37.43115146742694,
                          balloonX: 55.88776502507303,
                          balloonY: 37.43115146742694,
                          balloonContent: [
                              '<div class="ballon">',
                              '<div class="ballon_office">Альфа Страхование</div>',
                              '<div>Страховой Петя Владимирович</div>',
                              '<div class="ballon_numbers">',
                              '<span style="margin-right: 10px;">+7345-222-22-22</span>',
                              '<span>+7(543)-222-22-22</span>',
                              '</div>',
                              '<div class="ballon_email">username@flagstudio.ru</div>',
                              '</div>'
                          ]
                      }
                  },
              ],
              belarus: [
                  {
                      city: 'Бобруйск',
                      hideList: false,
                      offices: [
                          {
                              title: 'Альфа Банк',
                              fio: 'Какое То ФИО',
                              numbers: [
                                  '+7999-222-22-22', '+7(999)-222-22-22'
                              ],
                              email: 'username@flagstudio.ru'
                          },
                          {
                              title: 'Т&м cosmetics',
                              fio: 'Максимов Максим Максимович',
                              numbers: [
                                  '+375 25 768-50-57', '+375 44 776-61-22'
                              ],
                              email: 'megapuper@gmail.ru'
                          }
                      ],
                      geo_center: {
                          latitude: 55.88776502507303,
                          longitude: 37.43115146742694,
                          balloonX: 53.14679267916021,
                          balloonY: 29.229154783352502,
                          balloonContent: [
                              '<div class="ballon">',
                              '<div class="ballon_office">Альфа Банк</div>',
                              '<div>Какое То ФИО</div>',
                              '<div class="ballon_numbers">',
                              '<span style="margin-right: 10px;">+7999-222-22-22</span>',
                              '<span>+7(999)-222-22-22</span>',
                              '</div>',
                              '<div class="ballon_email">username@flagstudio.ru</div>',
                              '</div>'
                          ]
                      }
                  },
                  {
                      city: 'Мозырь',
                      hideList: false,
                      offices: [
                          {
                              title: 'Копеечка',
                              fio: 'Лепёшкин Женя Максимович',
                              numbers: [
                                  '+375 29 144-70-00', '+375 23 624-67-48'
                              ],
                              email: 'username@flagstudio.ru'
                          },
                          {
                              title: 'Супер маркет',
                              fio: 'Максимов Женя Максимович',
                              numbers: [
                                  '+375 29 324-66-66', '+375 23 624-67-48'
                              ],
                              email: 'gigantus@gmail.ru'
                          }
                      ],
                      geo_center: {
                          latitude: 55.88776502507303,
                          longitude: 37.43115146742694,
                          balloonX: 52.0501524366086,
                          balloonY: 29.265951299443376,
                          balloonContent: [
                              '<div class="ballon">',
                              '<div class="ballon_office">Супер маркет</div>',
                              '<div>Максимов Женя Максимович</div>',
                              '<div class="ballon_numbers">',
                              '<span style="margin-right: 10px;">+375 29 324-66-66</span>',
                              '<span>+375 23 624-67-48</span>',
                              '</div>',
                              '<div class="ballon_email">megapuper123@gmail.ru</div>',
                              '</div>'
                          ]
                      }

                  },
                  {
                      city: 'Минск',
                      hideList: false,
                      offices: [
                          {
                              title: 'Tempus',
                              fio: 'Лепёшкин Женя Дмитриевич',
                              numbers: [
                                  '+375 29 344-99-66', '+375 17 388-11-06'
                              ],
                              email: 'shilov@gmail.ru'
                          }
                      ],
                      geo_center: {
                          latitude: 55.88776502507303,
                          longitude: 37.43115146742694,
                          balloonX: 53.904156880963754,
                          balloonY: 27.552182381289317,
                          balloonContent: [
                              '<div class="ballon">',
                              '<div class="ballon_office">Tempus</div>',
                              '<div>Лепёшкин Женя Дмитриевич</div>',
                              '<div class="ballon_numbers">',
                              '<span style="margin-right: 10px;">+375 29 344-99-66</span>',
                              '<span>+375 17 388-11-06</span>',
                              '</div>',
                              '<div class="ballon_email">shilov@gmail.ru</div>',
                              '</div>'
                          ]
                      }
                  },
              ],
              geo_ru: [
                  {
                      latitude: 53.24447526078375,
                      longitude: 34.36597424339776,
                      balloonContent: [
                          '<div class="ballon">',
                          '<div class="ballon_office">Офис Брянск</div>',
                          '<div>Белкин Рома Евгеньев</div>',
                          '<div class="ballon_numbers">',
                          '<span style="margin-right: 10px;">+7999-222-22-22</span>',
                          '<span>+7(999)-222-22-22</span>',
                          '</div>',
                          '<div class="ballon_email">pupalupe@flagstudio.ru</div>',
                          '</div>'
                      ]
                  },
                  {
                      latitude: 53.25560784197745,
                      longitude: 34.34438916635044,
                      balloonContent: [
                          '<div class="ballon">',
                          '<div class="ballon_office">Офис Брянск-Полимер</div>',
                          '<div>Полимерович Сергей Александрович</div>',
                          '<div class="ballon_numbers">',
                          '<span style="margin-right: 10px;">+7999-222-22-22</span>',
                          '<span>+7(123)-222-22-22</span>',
                          '</div>',
                          '<div class="ballon_email">pipapypa@flagstudio.ru</div>',
                          '</div>'
                      ]
                  },
                  {
                      latitude: 54.51478669415213,
                      longitude: 36.264054381996104,
                      balloonContent: [
                          '<div class="ballon">',
                          '<div class="ballon_office">Офис Банк России</div>',
                          '<div>Банкирович Олег Тинькофф</div>',
                          '<div class="ballon_numbers">',
                          '<span style="margin-right: 10px;">+7999-222-22-22</span>',
                          '<span>+7(999)-222-22-22</span>',
                          '</div>',
                          '<div class="ballon_email">username@flagstudio.ru</div>',
                          '</div>'
                      ]
                  },
                  {
                      latitude: 54.51955999279444,
                      longitude: 36.26577688477283,
                      balloonContent: [
                          '<div class="ballon">',
                          '<div class="ballon_office">Офис Красивые брови</div>',
                          '<div>Красавчиков Дмитрий Олегович</div>',
                          '<div class="ballon_numbers">',
                          '<span style="margin-right: 10px;">+7999-222-22-22</span>',
                          '<span>+7(999)-222-22-22</span>',
                          '</div>',
                          '<div class="ballon_email">brovki@flagstudio.ru</div>',
                          '</div>'
                      ]
                  },
                  {
                      latitude: 55.88776502507303,
                      longitude: 37.43115146742694,
                      balloonContent: [
                          '<div class="ballon">',
                          '<div class="ballon_office">Альфа Страхование</div>',
                          '<div>Страховой Петя Владимирович</div>',
                          '<div class="ballon_numbers">',
                          '<span style="margin-right: 10px;">+7345-222-22-22</span>',
                          '<span>+7(543)-222-22-22</span>',
                          '</div>',
                          '<div class="ballon_email">username@flagstudio.ru</div>',
                          '</div>'
                      ]
                  },
              ],
              geo_be: [
                  {
                      latitude: 53.14679267916021,
                      longitude: 29.229154783352502,
                      balloonContent: [
                          '<div class="ballon">',
                          '<div class="ballon_office">Альфа Банк</div>',
                          '<div>Какое То ФИО</div>',
                          '<div class="ballon_numbers">',
                          '<span style="margin-right: 10px;">+7999-222-22-22</span>',
                          '<span>+7(999)-222-22-22</span>',
                          '</div>',
                          '<div class="ballon_email">username@flagstudio.ru</div>',
                          '</div>'
                      ]
                  },
                  {
                      latitude: 53.14679267916021,
                      longitude: 29.229154783352502,
                      balloonContent: [
                          '<div class="ballon">',
                          '<div class="ballon_office">Т&м cosmetics</div>',
                          '<div>Максимов Максим Максимович</div>',
                          '<div class="ballon_numbers">',
                          '<span style="margin-right: 10px;">+375 25 768-50-57</span>',
                          '<span>+375 44 776-61-22</span>',
                          '</div>',
                          '<div class="ballon_email">megapuper@gmail.ru</div>',
                          '</div>'
                      ]
                  },
                  {
                      latitude: 52.0501524366086,
                      longitude: 29.265951299443376,
                      balloonContent: [
                          '<div class="ballon">',
                          '<div class="ballon_office">Супер маркет</div>',
                          '<div>Максимов Женя Максимович</div>',
                          '<div class="ballon_numbers">',
                          '<span style="margin-right: 10px;">+375 29 324-66-66</span>',
                          '<span>+375 23 624-67-48</span>',
                          '</div>',
                          '<div class="ballon_email">megapuper123@gmail.ru</div>',
                          '</div>'
                      ]
                  },
                  {
                      latitude: 52.0501524366086,
                      longitude: 29.265951299443376,
                      balloonContent: [
                          '<div class="ballon">',
                          '<div class="ballon_office">Копеечка</div>',
                          '<div>Лепёшкин Женя Максимович</div>',
                          '<div class="ballon_numbers">',
                          '<span style="margin-right: 10px;">+375 29 144-70-00</span>',
                          '<span>+375 23 624-67-48</span>',
                          '</div>',
                          '<div class="ballon_email">gigantus@gmail.ru</div>',
                          '</div>'
                      ]
                  },
                  {
                      latitude: 53.904156880963754,
                      longitude: 27.552182381289317,
                      balloonContent: [
                          '<div class="ballon">',
                          '<div class="ballon_office">Tempus</div>',
                          '<div>Лепёшкин Женя Дмитриевич</div>',
                          '<div class="ballon_numbers">',
                          '<span style="margin-right: 10px;">+375 29 344-99-66</span>',
                          '<span>+375 17 388-11-06</span>',
                          '</div>',
                          '<div class="ballon_email">shilov@gmail.ru</div>',
                          '</div>'
                      ]
                  },
              ]
          },
      isActiveRu: true,
      isActiveBu: false,
  }),
  methods: {
      openList(country, city, i) {
          if (country[i] == city) {
              this.hideList()
              city.hideList = !city.hideList
              console.log(city)
              this.init(this.countries.geo_ru, city.geo_center)
          }
      },
      activeBtn(country) {
          if(country == 'rus') {
              this.isActiveRu = true
              this.isActiveBu = false
              this.selectedCountry = this.countries.russia
          }
          if(country == 'bel') {
              this.isActiveRu = false
              this.isActiveBu = true
              this.selectedCountry = this.countries.belarus
          }
          this.hideList()
      },
      hideList() {
          this.countries.russia.forEach(e => e.hideList = false)
          this.countries.belarus.forEach(e => e.hideList = false)
      },
      init(country, geo_center) {
          const boxes = document.querySelectorAll('.ymaps-2-1-79-map');
          boxes.forEach(box => {
              box.remove();
          });
          const map = new ymaps.Map("map", {
              center: [54.777801, 34.326837], zoom: 6
          });
          let geoObjects = []
          for (let i = 0; i < country.length; i++) {
              geoObjects[i] = new ymaps.Placemark([country[i].latitude, country[i].longitude], {
                  balloonContent: country[i].balloonContent.join(''),
              }, {preset: "islands#circleDotIcon", iconColor: '#1e345e'
              })
          }
          let clusterer = new ymaps.Clusterer({
              clusterIcons: [
                  {
                      href: circle,
                      size: [30, 30],
                      offset: [-20, -20],
                  }
              ],
          });
          map.geoObjects.add(clusterer)
          clusterer.add(geoObjects)
          if(geo_center !== undefined) {
              map.setCenter([geo_center.latitude, geo_center.longitude], 14)
              map.balloon.open([geo_center.balloonX, geo_center.balloonY], { content: geo_center.balloonContent.join('') });
          }
          map.controls
              .remove('rulerControl')
              .remove('searchControl')
              .remove('trafficControl')
              .remove('zoomControl')
              .remove('layersControl')
              .remove('geolocationControl')
      }
  },
  async mounted() {
      this.selectedCountry = this.countries.russia
      await axios.get('https://api-maps.yandex.ru/2.1/?apikey=6624ed2d-e91a-4eca-8ea7-7104d4b2b177&lang=ru_RU').then(res => {
          this.init(this.countries.geo_ru)
      })
  },
}
</script>
<style>
    .side_office_list {
        display: flex;
        flex-direction: column;
        align-items: start;
        list-style: none;
    }
    .side_office_list {
        display: none;
    }
    .side_office_city.active ~ .side_office_list{
        display:flex;
    }
    .side_office_email {
        color: #6bbcdc;
    }
    .side_office_num:first-child {
        padding-right: 20px;
    }
    .side_office_numbers {
        display: flex;
        flex-direction: row;
        margin: 15px 0px;
    }
    .side_office_city_icon{
        transition: 0.2s;
        transform: rotate(0.5turn);
    }
    .side_office_city_icon.active {
        transform: rotate(0turn);
    }
    .side_office_header {
        color: #34497d;
        font-weight: bold;
        margin: 20px 0px;
    }
    .side_office_city {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        width: 300px;
        color: #f0a119;
        font-weight: bold;
        cursor: pointer;
    }
    .side_office {
        display: flex;
        flex-direction: column;
        align-items: start;
        padding: 0px 20px;
        list-style: none;
        margin-bottom: 40px;
    }
    .side_country{
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        padding-bottom: 10px;
        border-bottom: 2px solid lightgray;
        margin-bottom: 20px;
    }
    .side_country_btn{
        cursor: pointer;
        display: flex;
        align-items: center;
        justify-content: center;
        text-align: center;
        width: 50%;
        height: 50px;
        margin: 0px auto;
    }
    .side_country_btn:hover{
        box-shadow: 0px 2px 5px gray;
    }
    .side_country_btn.active{
        color: white;
        font-weight: bold;
        background-color: #fe9d00;
    }
    .home {
        display: flex;
        flex-direction: row;
    }
    .side {
        position: relative;
        overflow-y: auto;
        width: 520px;
        height: 100vh;
        box-shadow: 1px 0px 5px gray;
        z-index: 1;
    }
    .ballon{
        padding: 5px;
    }
    .ballon_office{
        color: #c88730;
        font-weight: bold;
        margin-bottom: 5px;
    }
    .ballon_numbers {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        margin: 5px 0px;
    }
    .ballon_email {
        color: #3288b2;
    }
</style>