<!-- eslint-disable unicorn/prefer-includes -->
<template>
  <v-container>
    <v-row>
      <v-col
        cols="12"
      >
        <v-card>
          <!-- เดี๋ยวอาจปรับอีก  -->
          <h1 class="ma-2 pa-2" style="display:inline;">
            บริษัทคู่ค้า
          </h1>
          <h2 style="display:inline;" class="font-weight-light grey--text text--darken-2">
            COMPANY
          </h2>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col
        cols="12"
      >
        <v-card>
          <!-- ปุ่ม add delete -->
          <v-row>
            <v-col
              cols="6"
            >
              <add-new-company @addCompany="addData($event)" />
            </v-col>
            <!-- page shop มีปุ่ม delete -->
            <v-col
              cols="6"
              align="right"
            >
              <delete-no :selected="selected" @deleteByNo="deleteData($event)" />
            </v-col>
          </v-row>

          <v-row>
            <v-col
              cols="12"
            >
              <v-data-table
                id="headtable"
                v-model="selected"
                show-select
                :headers="headers"
                :items="companies"
                item-key="no"
                class="elevation-1 mx-3"
              >
                <!-- ประมาณว่าสร้าง el แล้วใส่ลงใน column action-->
                <template #[`item.action`]="{ item }">
                  <tr>
                    <td>
                      <v-menu offset-y>
                        <template #activator="{ on, attrs }">
                          <!-- ยังไม่เข้าใจเท่าไหร่ในเรื่องของ on attrs-->
                          <v-btn
                            depressed
                            v-bind="attrs"
                            v-on="on"
                          >
                            Action
                            <v-icon
                              right
                            >
                              mdi-chevron-down
                            </v-icon>
                          </v-btn>
                        </template>
                        <v-list>
                          <v-list-item
                            v-for="(list, index) in item.action"
                            :key="index"
                            link
                          >
                            <v-list-item-title>
                              <v-icon
                                left
                                :color="list.color"
                              >
                                {{ list.icon }}
                              </v-icon>
                              {{ list.title }}
                            </v-list-item-title>
                          </v-list-item>
                        </v-list>
                      </v-menu>
                    </td>
                  </tr>
                </template>
                <!-- ประมาณว่าสร้าง el รูปภาพ แล้วใส่ลงใน value picture-->
                <!-- ประมาณว่าแปลงเป็น el รูปภาพก่อน แล้วค่อยเอาไปแทนในค่าที่ value:'picture'-->

                <template #[`item.picture`]="{ item }">
                  <tr>
                    <td>
                      <img :src="item.picture" width="60" height="60">
                    </td>
                  </tr>
                </template>
              </v-data-table>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
//  blue darken-4 white--text
export default {
  name: 'CompanyPage',
  data () {
    return {
      selected: [],
      headers: [ // ประมาณ column แรก
        {
          text: 'No.', // ชื่อ column แรก
          align: 'start',
          sortable: false,
          value: 'no' // ชื่อตัวแปรที่เอามาใส่ใน column แรก
        },
        { text: 'รูป', value: 'picture' },
        { text: 'บริษัท(TH)', value: 'nameTH' },
        { text: 'บริษัท(EN)', value: 'nameEN' },
        { text: 'taxId', value: 'taxId' },
        { text: 'Action', value: 'action' }
      ],
      companies: [
        {
          no: 1,
          picture: 'https://www.gpfoffice.com/web/image/1085-d712b9c9/logo03.jpg',
          nameTH: 'บริษัทจีพีเอฟ พร็อพเพอร์ตี้ แมเนจเม้นท์ จำกัด',
          nameEN: 'GPF Property Management Limited',
          detail: '',
          taxId: '11111111',
          action: [
            { title: 'View', icon: 'mdi-eye', color: 'green' },
            { title: 'Edit', icon: 'mdi-pencil', color: 'orange' }]

        },
        {
          no: 2,
          picture: 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAOEAAADgCAMAAADCMfHtAAAAhFBMVEX///8CAgIAAAABAQH+/v77+/sGBgZvb2/4+PikpKRNTU319fWLi4tERERzc3Px8fHa2tovLy/Pz886Ojp8fHyqqqrg4OAPDw+IiIiysrI1NTXBwcHo6Oi5ublpaWleXl7T09MlJSVISEjIyMienp5WVlYZGRmUlJQ/Pz8gICAWFhZaWlqpIT6GAAAcNUlEQVR4nO1dC1fquhJOp0kpVgUVyqsgiIq6////u5k8J2la2OqBfddizt1ehTbNl5lM5pWUsStd6UpXutKVrnSlK13pSle60pWudKUr/cPEGef4E3/Vv8cXcP/P3sLsLb3NUmKn3KCeZp7F3fVcRG3RZnmix3HLQugL8f/lT/yPfo8fmM+5uwr/VPd1tsrN9RHKvn7I5wrO5MMF/iH7VHLSgTQJUR5DKAygUpRuxMILzFDpL+VlwmHo67NsN0Qs4oZbd6ifZVm6R+n2S8667xTYcH+7kkaPh8n9n7c/m8n8dd9ULLwFh5CJUbN6nU8mg5ubtz/39/PDqhnhIHe2WYpRRKU4hnC0kv14fnuT/RiOK8VV1VQTN0VJ9MA37a4GENDLvMZ7PMpytJjfQIveB8MRY12zi5cf4eVvomOszadi+BxcPxus7fhFHQxp3MdDKcRstZRX5VmWZ7n8KX/gXTdroQdcVKw5GHT4daYvlJSpz+5rJoU7KUXbqCezOj3WokS5rA54DTaOT0CSf34ajPd9COtOeGr0yo1stnDN4g9QvZ9b5TrQ4PKMXOKvlReWcsalNPABsoxcm8Oig9ulvHm/M8NM/hUFwMQgzNPPl//6EbJ6KfEBFIY9euwkwAyGus+CfUBRZB2Ez4X3MWMpOXmCAmiz8Cc9YaRCYa8A7cazbAZDi7CrC0cQjmaA/S8owhz7BTAyPGQfWngIqjzP7VjI8SlgNmaJZWMb3oZD0SQ7IafnHLtRxCjkCEHzA4ScV+UDqN6GIgAoURvheUhlJ89wQOwY68mrIQZty/umkEEoUbBOwZN3HpDb4Nv0M/GZW4TfkFI5ee4TsmEGZmr6rBBSwslhFILjKixHIQ9Rgz3FPMzgOdENudatomkSdOMHPBTsMe6Cvwtkl0UCIeqg3bLwilV9WMBtOMXkvTWOXoRwN2p3gotm14XQCul3pVSOsuWFlIHcad9c/vfMhUhKKUyaqhrVhx3OQQNBziEIp5i8dwqRlOIytGohlLwfQJH56/Qs14uGZHpSSgFoj7o1zd4zIlez+mkzmWzuFMqFVf+xlBrlxpob8BoW1OJCFkUppDduABynCxi0TDmB3aAPKLLMD7V5WMRD7CtdDztNpQ149SWf/r5WMjSqhy8AletrG6HGXks16h8LX4yah4KPWlMch3tX8thYZW+Q+3bkMEjF9Tyfrg8DOZEb7nhI+5At1ou1pUXViRCc9kKBgL2yLrAH5XrgLmpJqUHI2dzLOD71kS7ngi0A/E1eN+5j00A8BuYEzHIY1Lqp0WpSGls9lFKAoIUuY5CNQ948MeW7oE9UstLLWxcPpQbMvJBn8MqIBVyyZ3dX7n/LpYkSKV228czGZmZwQP2qPDQ54sJabQEPAX0m4zGystOFW+BEsloftaFxckMnuIuHjFeoAK2eKlApkLuEExAccvOLBPtZRgPeeF6jqSE5qH1V7QFbavEwHqckDQNtjkOXom4eCjrTcngIfO2V00MwewHPIniMEA7pSoEKr05ZgDEPU2Zwm17BL9ty+Jbpe3oQ7gKrswh6tnHfweeBXCdFJXzOE9CBKlCMEy7ndxFmflGWEBMmFeuRUiY+gKgRmFHbVIBdsnJ42oI3C+E9RCi8/5GrZaBWnkYCYZ+UdiN0TJQAC9gyEUq/Q5jgoZzqT3Rg1XJhgg+4xhWmN5IrJZFDkE+xhJevIcuIToKPtObv5mEf1CF1bpQ/i8u8EOEzuqW0mgVS+m41EAaHJq5ttGOIiZdLneshotdUBPjn6b62EZaqp1Ln9yBcy6bz0Jm9bxiPokXdulRAIKUfNh6Jy5NUH3YJnNWyf2TB+yRN43KvVhOjS7OOqdKWUrNWsN7ITxNZHWhVvb+OWAJhioc8Wk/nVmlLgI++aViq+WD/ch6faoKzmQkwaCqgZZo7hCEPPXWHwhjbhca/MpjgCzFSCzOBEKEI0m/1+cLxVpk7dh4i8m2gLZ2Yc8Vs+VyCsIMlMcLBxtG4GyCfQkalVPnucrI/TJV7RxF6KUVjmJdo+Yx2zrTOiZ+jDJIHb9Apd8L5AlKjwZ2NEUqTpSGhJ2z8oRth6Ft42vfwsIK216WiP88N0fuRf4gIVUh4Al68JKteXLOllF97C2DkQs02e6GKj5gBFJK7QQ+SLnKbh7Q7fQhT4Z+8UD78155cFSFUXjcfDyAWUkNycG79V3CDpu7Uu2kFtlBaz2wVedeDk6T0ZISCP9jViCpLVGnY4WScRtrO69Vi/qxsLWuXyus/fM6GV3e+TWkMynZUUEr7SJB5C1Y46103XnQtFp1xmn6Ecl3AmG0Y4jITElexZBTDhQIyq+ML5bLuhW+2duzN1QLPtaNmnyOdT9005yEPu43jb/IQ3Y6VQRgPkLyzTsfaEr9I6ToIorR9JBhDVIphb25RQClZ2yxaxENAB+cXeSj1YclW7+loVG6Cze1YW3xhpgPTfgKJNzINn3VSUE5EJyrSQdLKtMXDHDZ/ycN+XcoVxPoFWlE/pMIEeHoQKt6j/p8HGceaeMZSq6CPKk0AbyHKcS95EmF2OkKrNPqllOnM4BDj3tb+dSKQwb1HmJbSTIeN3kkADQ0pawmoDoyNVimMf6HHZGUvriEPpPSX10MkjOJUQ9DufjBIOIX6eZipR00a4o6g2f5MhPSjNN/dEMYWZgZIZV5B1NppNs3sdappOH3tz8vYtD1/BQizL7hkjPsR4gAWh4bRuIQU1oZEgmFgF4ap/1Ai/FJ6yVht9KE+xteL0NulR91ElSBH5d08axPOy2Oh1/aWlPp04PNhi88gHis6JlO7/oBOVGiu7MFpTZSylUW4A/JIONm3sGl9kUx6JQmzI9gHGkCdOIR0/DavkqarpMGL09AKKcaViLH6ScMxKuSm4mUbXEZc22inxxFVizDk4Uk+fkDS0pzQRAMO1H0S4dRm3dLUzBzCApbrlaH9kqYm4GEkuIIyhIw0L++oYgf1txDyijUFZFQetepOeMAqEtAVt1qDM3ayIPhOVnz5v60JWT7SxuU18MjLNMJASr/Fw4o9BDykCD3lmBsuu+pGOPdBtiD/hvJP/ipUmkOV40TZV9gkA7zf4yHGOYRNp1Qq4UceJWUsLaVD1l1Fw3lDQ4w+Wp/TDiIXPipTzPQcKnFpyGIBEHbLFCf9ACEv5WIlTFqjkkx890u14uHcIQyltK/1vQthx2ZCTj8FFdhDivMbcKOreeQ8VTxIZddOllLBJzU3E1sK3hDAjzWmBBedPOxu877XiCXN2JRqE9mM0mptVIWbhChG7Gc8FNLNud8ap6Ccgl317TDXDuGpPOSjhzQP0Tmk9lkOL2Vp00qZ/s58XcDdSllT1Xhwa33ldu7ppHmIEgJwN39d7BeHt9Bnl80sK4fwFB6q2bkPtJIpLZJUFM7ytWwYa+u71CEPu5RoDbwbzOf3S2mmjliahwJnl6Xk+qIJueO0eSRGhfFGT0eI105IqixM1CoPhvTTNK90TZ7PvG9lirK0l/2YzpDC0/JpaekzmctRjY9IFVKhc+7UNms8wlOkFBUgBtmc4oDNIqAbldc3zUiNopsXwoYE2uoJHc9UhjQPnAuoO6svpyaxpR6YEa2tRvHWxsNO5SGNBKsmH1uP8zpF/q7tvqpit0Fk3xPOFBMhjvxD0Nl+E/8csxRE7I+tNcj1yHogOBd2tBbjNIQufqFtUqBJJMFU+Yk3TbX5h+5xybE8LIEQL3pMIsTp6nuUNJFVOhvCgj33izK7x7Y28VQpxSzEztl9Ugo2UbWbuLNJ41yF99+YTabr0htaQWL6YQv32nEaq3yxqaSHiN1ftyp67MjpVG351zzcknRFrqqqgisH1Cos4L0WJl6DRVGm+CYiK6Z/XzGETW+CoDWVAIA9L8t0NLFPlx5cP9C4bqRGD65cQ2C8YaZDmN6sVEFGC4WUhMdvIhTCaNLM1Rnq5UvpqQdVa2iCmhJhTkj5Fh083LkrZSPLlvlao2azzcip/ocJH96XpoIKx5nKR3fRvUGY5V3UKaXi9ROUOkIGypmegyo6lJ8dRqRvyEMCEVyZUhth7S+0Vm34ULVeuIbgqyFrdTMHrdpzG1rNMRX3YXkYDHMAEOrUcojpIzZ6tbXbZIGZTTCT7tVvyZbhuv3aqWkOwXWJeMQkMgGmQbK6Hsys7WHX/JdppS/YxNZDsB4mC3MVRCbqxf3bcmYv/bzbLBrOgm0Ugr/ObwnNW1VNjoZzcuUkkbnczsOm3ETE58hBrdebO9uZ4untsC2tMC3COwOaNKlpY/agKPaO6u32cYuElebKL/UrGW+nkTv8+9g4bCNsm4/BSKJLyMp6vNV9qfSHRqH30dGdKle60pX+dVLqqGtfnIh2xqngg9myVpp/XTYeFvtgpEnogkimSz/VzWeC5rrSuzMuVnMVL6nOFLxKO+AmzBfXUOKzjm3/+m06khcIPTOutgLW6+Hk7ebtZnOY7rvqeUy11mi/WhwmG7z6ZjCfrlWy/MwI2RBmnQQrHjufj/PPwOB4W3R1eLvYvLcNlIf5qq/K6b+gYZ/BtPLmBJoKbHVn7C6TxVKW0mvFsMaOFCFx8Th/ME3o6/QNxrS6Hass1tGtob+GMA/9UhrWIwilETPC/YOxVy0/2q0ZBsfchby5M/Znu02MpwDMR+x8KmfY55oRhNID2kHR3uamYm63vCTF8apoKu/YCCQ/lrbp8pFVJ6cGz4QQA/qpXXx5prcRUikdF2Fw1tS22cjEDO+Y9ldW/jLCo1JqgoJFntqJifyCA3Wqx3bTqr4E9wfq+Bf+muv0MbZ9Hpv6FB5KgKMH9MjTl6nA1Jr4AGMagM7NRqACfV7ziZLtBpeqMyE8xkO5zE/s+Leuy3U2Ytf4bUNjoPFn9OVnOTghV6HNYoYx47Nom5iHNF7kEKoiWcNCsg/OB4BnmYtfs4iHkn/T7Xi8Hy6DeOoMdw+cstP+txH6RAdZD3n5AooZuQl3w/J1MaCaB/SeJ5FAmLvq22GQaAN4EWdDSCPCB5KKmNammGalqroUeyW7TBXl6MlC1nc+u8L6SErBxiHmQSmoKmw8gz4NeZhDEzoTOqj6RkcfQ4AqgVt/RQk1kdI0HqHK6tMvbr9RkfBThBh/9euU3X8yJrkVU46vBGwSdtjNxA6EjGbn8IvBWRyNWEprRoOpegWY6Np8i3GjHUUhxuAL9jBIbPcadEgprVVRDT2MzrHot3kY7VzifPQUqghd8415+KUvDcAVw+bbOngY75h1hwJcFqHUj7FqfOQGOtnCZsS0F2HJ6hjhuXgYSmlr79crFa0MdiNbxs1u7Zqope7OI0xKKW9CKUWz5jwIQx6avTEGaJtTH1Yz6gwe+cYeG9GlacSo+AektBH28CKbGzO7D5yUPvtu1WFm207EU6W0fULBf4UwkFLuISqAvA6dWSytbCE0X60dwpSUcq1pvJQOEpmA/wYh5RA5PEEFVAQPOZLT/FvzHpoCE4cwxUPOFsFhFJicusB6OLm9nSu6nex1t/b06BxklF8wsZCP8vDNIUzzcEObwgMHLjAPaSmL3gLFphGX975bo6dwIXlxCGMeKrEnBzHgBqlDck/wf44wECKF8DbSRQRheeOhoAAsOxGq/yZA9yI+CHGWwGKHB5xbhEwjJIsYOT+iuvFSqpL7DmEspULO6ldQTrT9FGOK54goHufhICo62/pluiQnoPTyEOkgHXsTj8rVhiYX2bk0wk2E8JEgfAkt1ncVe4kRZq/7/UIn+XXUBmNtK5Yq+P6vEPZJaaQAVR2zu9khNJK300cXhVLqNvzlJvIm8d00QpwTYT8PJxEPqaZ5CXYb2MMzohW00PEOEgNZBMUE50Zo/ww1DQG+J5H+SEqX2iAPEZomCysDy0dd4n2piHC8Hgq97Zes+ISHzQNd8bO0LjVBOhujgWJcVbre7XwIKYcGE0v3eveSNLVC4SWHBzafIQ+TK34G1DmBQh3DdcYkYo9dakr7HqMrDqHl7b9xe0RjS3b/7psoVLneOVPdoZTqilydp7e+xbjPt8iIB+x2MkdSCs3cnpWjp+LNWfPAiVibcX2dY74LZe6P3/u1J7tjcVfTyiEM2xwTPzLPgszreRD2RTHkAncTRjGezDcCd8zmpEzdHU4T87B2u/R07S+8nDPTfTTWFm+UwWVdQZfaYhAcTOF89thqqzkmH+lGhkW7I5dDqPxWf42r68WdRshdsi34hqUQYi26XDiB7Gfv3PL8XyHslVKdHHX7etT2WgxxlKVwOkjZKoU7Xyj2LWrc4ZWRow8w3hGfe/afIuzjIZphf0Lb7EbF9MsyPDAt92dExauFbLPaBXsuoOjcxXl+hAI3ixAfqVDehZAyWgY7GjO4s/qjjZDz1xChOsPvfAj7dSlnJsypLkHPQG0Ks7ugtDzmqtOlQxj4FoiQ7lFSDD9LFMoi7OMhHmBIww/Kdl2uSr6l27wy7Rwms2sKoeAYH/clKEBNo8siVFQHKzY6fJ87kuaWjM1nJBuYklLeoKjTk83S5yr/Nwj7pBQJD3N28mg76LYU4l9SdovaZQNTUop783N3GC1KdddRLv8FwmM8lLPoPbysUMq1cCd+FbhUiB4eioptMblKPj0XE0+SUiHWEFXT5IXbdISVQHBTBdUmLSktOaP5D9RY8/O4GKdJqVBnf2U0YENsgAJmM7p3p2WXclvvQGVXnRB8kexaAiGGjZ5hli6KUqGlFe1rm4f6y6/o+Pv7s1V9HeOhqs6WEMMERkbUzjQQuFZEmNtn0YCI5OL4HD7GMORLUKngEKpjQcWErA8q3mjLaL8ewxMn2xFhHS4oI4scz74/B8KC7huDumMhVinfB1udhnWm+iBGSYPwXB2V+A93n9mAyDzYoqY2Qp4DYUjjLgWHNbLldKerf9UBbfqG5y2rAmFTCAOy2bWqicusb86gaVb38wmhQdNRL8jNfsntgW7n+zNsWvkVwepJQPfmnMaKs0P4xf3gyDFQv0AJvdlhEZd6twT+2uzX0+l0tUWXnuMWBRG10GrTviWo1eq5tyZc6UpXOgPpvbMqRmJP+3W7uNoq9OiuD3qgVGvPFqdvNSjNt0fLgtU7A47i6Lnf7Ltr9ZzzxDvFeHnEcAy274kIIhf0bleAdCR4gUdp/ASheU7TjBeH+b2m+eu+bkSqbr4sK/XCrSRpEOT7qqrCVUN+1PpSiO4WNXW/z+JE4s1qvnmIbYv3t8m+XWS2uemmJ7XRjlfP7pOXm5dpgJCV9IYXfUbJ4aWnUUl3+9NPEmxhk88cTe3mfm8e6tIIgM9JrTfrO07cxQNBSZdgVjv6WVEzOjFZcIMOlcfHErRo9X2/WE411XzgD1I3Do+lFyUxMu+Sh7sax0AjqR6CoqgNmUSckaMvc4Kwq0ntb6y+nx3mvFImZV/zOIIl5WHHDrQ0QnWk8lGE7TNOfguhECM8qQWoD2teGKg8W70L7ZW1eEgcZJLvDxC6Tz95hNDcFCEMfWjqgf8EoSq0y8l5h8pFy3O9q0yPoN6FZifTX/Mw1y8psSfvnJuHsjtPUbwsy0yJkttNMcv1VBffQpjlM2jsjtnLIXTCYU9cU9sDrZRIkCPhefiXUlrg+VCiJDw8r5SGPMxhsB+P968v9KCzTMUxzQtq78IX/YRBuSQPsZhkbaoq/06XuqDWb0qpLXpaBVkJibE2xiNdLfIiPCeuA6EcoIeK9Uqpp2C4f2E9jKXUlXWxtZx+7tMc3MsRDQ/1VwV8zglNOEVI4sQ5HOx+hpSULsz9t7Nw59uLKb6eb/teDnwaQj9sBqGgx3FnemNTi4ekCCNo8iFeX/PMxOuSPPSdD8PL/qUCnWGUHyBkYh0GOB9GPJ6HKKRP4bkgHQjl/W/dUuqO4WBR7PRWv+KZH3l7zkkIW1Iq+BaoHeBenBBIaaYPMEsjDNSiLibhSSnVrptGSG+CW1OLhS7Y9yPhXTzko6jsaezWQ/Kh5GGiyYSU4lF62gdKaBpHoU7VrwL5MXVpGjy0jQwoLvp0xbfJotSr2xI8REtpridTQtNECB0PfxNhi4ecx0cLmgLLQNPkp/IQ34CG5RqX42EbYRUf6r1n7RUf+ngYktRKfwT/h6S05KGUgk3PUKstDzWNVXgpTaNKT6ZGX/4TUioiTYOrBTMIaTHaE3XfQ4QtgvdGzsR/RUpZtD2Nrvj0fQB+xfeLVgfCXJ9q+a9IKZ6LbT9VIZs9S/gWkO0mzmYbjBLrYQ76aHblTKtzhv8FKRXqNXM3lleyazN4s6f7R/4htbttpUjAwwLPF7YtwUfJyTuXLyalXG/ndf4TzPCwcWMadkaiUgjlhbMvIu0YEJldBiGVUhXp3n7BzJyLjeanxm0Q0vdheWXZQqiNtaeBOvXZWgjvDbh3BV1Sl4r1e1aYImychXO/y/mveIjG2pY4ffiqr5dL69Lb8XgrXXxTA5KrwphbfC8Db68WpyCc1RNaUwtfHxfWpTZOk2vlh3Gb9wXdz/J3Uooh7wb020/1tVgpdkEpVYWCBf0D3icNZmPck+4gvZyneZjjW1cP9Fge8grSy/BQFWzpOBvyczZtdJKNIiS+BXmVYpKHOaYtRupI7Mwuo9kleZjpl/fkVtup1z4KzsgOcqppQJ2l79fDto+veMjUCwntAwpq9J1f00jdFy7ou/idYZHV9vXn2dKbTcUFCPG14uxNx1/1RxeWUpiYN5cYs6x1Wjm12gr10mNHqSiG5CFXB5sXwd7Zy0lpBvM1HcgcvqowREJ5KCVz6c5y9W9GCaV0zCvcv584ffBCUjoXD1D4zSJYPx+cnh1IaUF9fDdZW/OQs3IX1xNfSkrxbHyp3HO/gKl3+bKWprFCfCxOg/MQae2NtYtKaY5vehhhPs19VKiX3rE0D4/GaTQP0Rn7015GLySlUzwkLgjFqAMekqvFKQg1D0teQyuFdiEplb5FTT/RL0X0Iee/jCZaHuKryQv3QuHLSanxLZ6pNlVvk/gpD6UQjJaxPr1gnGYf5Q/uWctqOxnhzCBUR8+FXskl4zRP9FWh4dElgZQe1aVg56FEKJ6h+EekND6RzBzl5bNr5Jsj2TXCQ94+OOJiPJS2yTt4HuJo7p3JEmma96AgvJ3Ht+shosR3tuQkY38xHiIS+rJgND//MBZWKmS2F/1ZbstDjbBaQhZu5LpUrE2/eNlbbkUGrnauFU2kuygSCD0P6TmXl5ZSzvWq70UGXiqRyFtE6j9RbUJ5KNXNs7r70lKqKDy/Q7Jqnaw2Ca2UIzxUzV7aA/Z5i2eY0S/gqfxO1RfloaJbyC6DMFoPpUg9gn9/npKoofYT/7ImKuIhLz//hbwFEuYtyHjmmF5jBOGJPISYh6oUyX3ZRhjIza8gVMfl0lan9v15q+BpEuKruuEFYv0SIRRYsmo/KaAIXsbIS+lG5e7LNsKgudtf2J8vuxPVd9u3jJfiIypF/lTRwvjT9noYNRm+brJCN8pTgDCqkAZ8gfCPecjFaDKgtHEvBmCLzSD8Sp1EOok+pfRseBg0GUgpBs+HvoVNAF+U0QOHXPz8iMHEhgq7Q6DV9mkSE28PCf4qS152ftnaN4Inbfycie39Ibz9bPrtse0r9PzRduum/sn9Hj04bvq6Ue9KV7rSla50pStd6UpXutKVrnSlK13p/4f+Bx8SexfOi34QAAAAAElFTkSuQmCC',
          nameTH: 'เช่าพื้นที่',
          nameEN: 'rent zone',
          detail: '',
          taxId: '00000000',
          action: [
            { title: 'View', icon: 'mdi-eye', color: 'green' },
            { title: 'Edit', icon: 'mdi-pencil', color: 'orange' }]

        },
        {
          no: 3,
          picture: 'https://images.unsplash.com/photo-1536256263959-770b48d82b0a?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=387&q=80',
          nameTH: 'คามุ ที',
          nameEN: 'Kamu Tea',
          detail: '',
          taxId: '123456781',
          action: [
            { title: 'View', icon: 'mdi-eye', color: 'green' },
            { title: 'Edit', icon: 'mdi-pencil', color: 'orange' }]

        },
        {
          no: 4,
          picture: 'https://images.unsplash.com/photo-1596428361862-a11618414833?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1170&q=80',
          nameTH: 'สตาร์บัคส์',
          nameEN: 'Starbucks',
          detail: '',
          taxId: '123456782',
          action: [
            { title: 'View', icon: 'mdi-eye', color: 'green' },
            { title: 'Edit', icon: 'mdi-pencil', color: 'orange' }]

        },
        {
          no: 5,
          picture: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTbNwpfwy6-eD3wI_0180r0fUyf36kV-WKciVpOPQ8l_7TLrR5SKS3WaVcC9sXfH1OPySo&usqp=CAU',
          nameTH: 'ธนาคารไทยพาณิชย์',
          nameEN: 'SCB Bank',
          detail: '',
          taxId: '123456783',
          action: [
            { title: 'View', icon: 'mdi-eye', color: 'green' },
            { title: 'Edit', icon: 'mdi-pencil', color: 'orange' }]

        }
      ]
    }
  },
  methods: {
    addData (companyDetail) {
      const newCompany = {}
      newCompany.nameTH = companyDetail.nameTH
      newCompany.nameEN = companyDetail.nameEN
      newCompany.detail = companyDetail.detail
      newCompany.taxId = companyDetail.taxId
      newCompany.no = this.companies[this.companies.length - 1].no + 1
      newCompany.action = [
        { title: 'View', icon: 'mdi-eye', color: 'green' },
        { title: 'Edit', icon: 'mdi-pencil', color: 'orange' }]
      this.companies.push(newCompany)
    },
    deleteData (no) {
      this.$swal.fire({
        title: 'Are you sure?',
        text: "You won't be able to revert this!",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, delete it!'
      }).then((result) => {
        if (result.isConfirmed) {
          // eslint-disable-next-line unicorn/prefer-includes
          const result = this.companies.filter(item => no.indexOf(item.no) === -1)
          this.companies = [...result]
          this.$swal.fire(
            'Deleted!',
            'Your selected items have been deleted.',
            'success'
          )
        }
      })
    }
  }
}
</script>

<style>
.v-data-table-header{
  background-color: #0D47A1;
}
#headtable table thead tr th{
  color: white;
}
img{
    margin-top: 6px;
}
#container {
    display: flex;
    justify-content: space-between;
}
</style>
