<template>
  <div
    ref="container"
    class="container relative mx-auto text-gray-100"
    :style="{ 'max-width': '960px', 'background-color': '#2c2c2c' }">
    <Modal
      v-if="content"
      @click.native.self="clearContent"
      :content="content"
      :clearContent="clearContent"
    />
    <transition name="drop">
      <div
        v-if="inView || showPreview"
        class="
          fixed
          top-0
          w-full
          z-30
          bg-gray-900
          justify-between
          items-center
          flex
          md:hidden
        "
      >
        <div
          class="logo w-16 m-4"
          v-html="require(`~/assets/icons/logo.svg?include`)"
        ></div>
        <button
          class="
            p-3
            mx-4
            font-extrabold
            rounded
            tracking-wide
            focus:outline-none
            select-none
          "
          :class="showPreview ? 'bg-gray-700' : 'bg-emerald-600'"
          @click="!opening && togglePreview()"
        >
          {{ showPreview ? 'Close preview' : 'Open preview' }}
        </button>
      </div>
    </transition>
    <transition name="fade">
      <Preview
        v-show="showPreview"
        class="fixed top-20 w-full bottom-0 z-20 border-none rounded-b-none"
        ref="html"
        :username="username"
        :genInfo="genInfo"
        :images="images"
        :featured="featured"
        :colors="colors"
        :primaryActions="allActions"
        :secondaryActions="secondaryActions"
        :PreviewMode="PreviewMode"
        :downloadVcard="downloadVcard"
        :footerCredit="footerCredit"
        :showAlert="showAlert"
        :hasLightBG="hasLightBG"
        :downloadKey="downloadKey"
        :pubKeyIsValid="pubKeyIsValid"
      />
    </transition>
    <div class="px-4">
      <div class="pt-2 mb-4">
        <img
          src="~assets/images/DBB_WHITE.svg"
          alt="DB&B Philippines Logo"
          class="w-48 mx-auto md:mx-0"
        >
      </div>
    </div>
    <div class="md:grid md:grid-cols-2">
      <div class="px-4">
        <div ref="create" id="step-1" class="pt-8">
          <h2 class="font-extrabold text-2xl">Header attachments</h2>
          <div class="stepC">
            <Attachment
              :content="images"
              type="logo"
              :resizeImage="resizeImage"
              label="Add logo"
              description="suggested format: svg, png or gif"
              :showAlert="showAlert"
            />
            <Attachment
              :content="images"
              type="cover"
              :resizeImage="resizeImage"
              label="Add cover photo"
              description="suggested format: svg, jpeg, png or gif"
              :showAlert="showAlert"
            />
            <p class="mt-6 border p-4 rounded border-gray-700 text-gray-400">
              Recommended cover photo size is 960 x 640 pixels, with an aspect
              ratio of 3:2
            </p>
          </div>
        </div>
        <div id="step-2" class="mt-16">
          <h2 class="font-extrabold text-2xl">Contact information</h2>
          <Attachment
            :content="images"
            type="photo"
            :resizeImage="resizeImage"
            label="Add profile photo"
            description="suggested format: jpeg, png or gif"
            :showAlert="showAlert"
          />
          <p class="mt-6 border p-4 rounded border-gray-700 text-gray-400">
            Recommended profile photo size is 320 x 320 pixels, with an aspect
            ratio of 1:1
          </p>
          <div class="stepC mt-6 grid grid-cols-2 gap-4">
            <div>
              <label for="firstname" class="ml-4">First name</label>
              <input
                id="firstname"
                spellcheck="false"
                type="text"
                v-model="genInfo.fname"
                autocapitalize="words"
                class="
                  mt-2
                  px-4
                  w-full
                  h-12
                  bg-black
                  rounded
                  border border-transparent
                  transition-colors
                  duration-200
                  focus:outline-none focus:border-gray-600
                  hover:border-gray-600
                "
              />
            </div>
            <div>
              <label for="lastname" class="ml-4">Last name</label>
              <input
                id="lastname"
                spellcheck="false"
                type="text"
                v-model="genInfo.lname"
                autocapitalize="words"
                class="
                  mt-2
                  px-4
                  w-full
                  h-12
                  bg-black
                  rounded
                  border border-transparent
                  transition-colors
                  duration-200
                  focus:outline-none focus:border-gray-600
                  hover:border-gray-600
                "
              />
            </div>
          </div>
          
          <div class="stepC mt-6">
            <label for="job-title" class="ml-4">Job title</label>
            <input
              id="job-title"
              type="text"
              spellcheck="true"
              autocapitalize="words"
              v-model="genInfo.title"
              class="
                mt-2
                px-4
                w-full
                h-12
                bg-black
                rounded
                border border-transparent
                transition-colors
                duration-200
                focus:outline-none focus:border-gray-600
                hover:border-gray-600
              "
            />
          </div>
          <div class="stepC mt-6">
            <label for="business-name" class="ml-4">Business name</label>
            <input
              id="business-name"
              spellcheck="false"
              type="text"
              v-model="genInfo.biz"
              readonly
              class="
                mt-2
                px-4
                w-full
                h-12
                bg-black
                rounded
                border border-transparent
                transition-colors
                duration-200
                focus:outline-none
                opacity-50
                cursor-not-allowed
              "
            />
          </div>
          <div class="stepC mt-6">
            <label for="business-address" class="ml-4">Business address</label>
            <textarea
              id="business-address"
              v-model="genInfo.addr"
              readonly
              class="
                block
                mt-2
                px-4
                py-3
                w-full
                bg-black
                rounded
                border border-transparent
                transition-colors
                duration-200
                focus:outline-none
                resize-none
                opacity-50
                cursor-not-allowed
              "
              rows="2"
            ></textarea>
          </div>
          <div class="stepC mt-6">
            <label for="business-description" class="ml-4"
              >Business description
            </label>
            <textarea
              id="business-description"
              v-model="genInfo.desc"
              readonly
              class="
                block
                mt-2
                px-4
                py-3
                w-full
                bg-black
                rounded
                border border-transparent
                transition-colors
                duration-200
                focus:outline-none
                resize-none
                opacity-50
                cursor-not-allowed
              "
              rows="4"
            ></textarea>
          </div>
        </div>

        <div id="step-3" class="mt-16">
          <h2 class="font-extrabold text-2xl">Primary actions</h2>

          <div class="stepC mt-6 mb-6">
            <div class="mt-4">
               <label class="ml-4 text-gray-500 text-sm uppercase font-bold">Fixed Company Details</label>
               <div class="flex items-center mt-2 w-full h-12 bg-black rounded overflow-hidden opacity-60 cursor-not-allowed border border-gray-800">
                  <div class="w-12 h-full flex items-center justify-center bg-gray-800">
                     <div class="w-5 h-5" v-html="require(`~/assets/icons/call.svg?include`)"></div>
                  </div>
                  <input type="text" value="+63 2 7745 8800" readonly class="px-4 w-full h-full bg-black text-gray-400 focus:outline-none cursor-not-allowed"/>
               </div>
               <div class="flex items-center mt-4 w-full h-12 bg-black rounded overflow-hidden opacity-60 cursor-not-allowed border border-gray-800">
                  <div class="w-12 h-full flex items-center justify-center bg-gray-800">
                     <div class="w-5 h-5" v-html="require(`~/assets/icons/website.svg?include`)"></div>
                  </div>
                  <input type="text" value="https://www.dbb.com" readonly class="px-4 w-full h-full bg-black text-gray-400 focus:outline-none cursor-not-allowed"/>
               </div>
               <div class="flex items-center mt-4 w-full h-12 bg-black rounded overflow-hidden opacity-60 cursor-not-allowed border border-gray-800">
                  <div class="w-12 h-full flex items-center justify-center bg-gray-800">
                     <div class="w-5 h-5" v-html="require(`~/assets/icons/location.svg?include`)"></div>
                  </div>
                  <input type="text" value="https://maps.app.goo.gl/9JPriTcXZixMACVn9" readonly class="px-4 w-full h-full bg-black text-gray-400 focus:outline-none cursor-not-allowed"/>
               </div>
            </div>
          </div>
          
          <draggable
            v-model="primaryActions"
            handle=".drag"
            animation="1"
            ghostClass="ghost"
            class="border-t border-gray-800 pt-4"
          >
            <transition-group type="transition" name="list">
              <Action
                v-for="(item, index) in primaryActions"
                :key="'item' + index"
                name="primaryActions"
                :type="primaryActions"
                :item="item"
                :index="index"
                :buttonBg="colors.buttonBg.color"
                :removeAction="removeAction"
              />
            </transition-group>
          </draggable>

          <div
            class="mt-6 border-gray-800"
            :class="{ 'border-t pt-6': primaryActions.length }"
          >
            <input
              spellcheck="false"
              type="text"
              v-model="filterPrimary"
              placeholder="Search an action"
              class="
                px-4
                mb-2
                w-full
                h-12
                bg-black
                placeholder-gray-600
                rounded
                border border-transparent
                transition-colors
                duration-200
                focus:outline-none focus:border-gray-600
                hover:border-gray-600
              "
              @keydown.esc="clearFilterActions"
              @keypress.enter="
                filteredAction('filteredPrimaryActions', 'primaryActions')
              "
            />
            <div class="stepC actions">
              <button
                v-for="(action, index) in filteredPrimaryActions"
                :key="index"
                @click="addAction('primaryActions', action.name)"
                class="
                  p-3
                  flex
                  items-center
                  shrink-0
                  rounded
                  hover:bg-gray-600
                  focus:bg-gray-600
                  transition-colors
                  duration-200
                  focus:outline-none
                  bg-gray-700
                "
                :title="
                  action.name.substr(0, 1).toUpperCase() + action.name.slice(1)
                "
                :aria-label="action.name"
              >
                <div
                  class="w-6 h-6 mr-3 shrink-0"
                  v-html="require(`~/assets/icons/${action.icon}.svg?include`)"
                ></div>
                <p class="whitespace-nowrap">
                  {{
                    action.name.substr(0, 1).toUpperCase() +
                    action.name.slice(1)
                  }}
                </p>
              </button>
            </div>
          </div>
        </div>

        <div id="step-5" class="mt-16">
          <h2 class="font-extrabold text-2xl">Featured content</h2>
          <div class="stepC">
            <draggable
              v-model="featured"
              handle=".drag"
              animation="1"
              ghostClass="ghost"
            >
              <transition-group type="transition" name="list">
                <Featured
                  v-for="(content, index) in featured"
                  :key="'content' + index"
                  :featured="featured"
                  :resizeImage="resizeImage"
                  :index="index"
                  mimetypes="image/jpeg, image/png, audio/mpeg, video/mp4, video/webm, application/pdf"
                  :showAlert="showAlert"
                /> </transition-group
            ></draggable>

            <div class="flex mt-6">
              <div class="flex flex-wrap items-center">
                <button
                  class="
                    p-3
                    rounded
                    bg-gray-700
                    hover:bg-gray-600
                    focus:bg-gray-600
                    transition-colors
                    duration-200
                    focus:outline-none
                  "
                  @click="addFeature()"
                  aria-label="Add section"
                >
                  <div
                    class="w-6 h-6"
                    v-html="require(`~/assets/icons/add.svg?include`)"
                  ></div>
                </button>
                <p class="ml-3 leading-none">Add section</p>
              </div>
            </div>
            <p class="mt-6 border p-4 rounded border-gray-700 text-gray-400">
              Supported media formats: jpeg, png, mp3, mp4, webm and pdf
            </p>
          </div>
        </div>
        
        <Download
          :downloadCheckList="downloadCheckList"
          :downloadChecked="downloadChecked"
          :downloadPackage="downloadPackage"
        />
      </div>
      <div
        id="preview-container"
        class="relative w-full mt-20 sm:mt-0 hidden md:block"
      >
        <div
          id="preview"
          class="
            flex flex-col
            items-center
            justify-center
            sm:sticky sm:top-0
            md:mx-6
            lg:mx-12
          "
        >
          <div id="device" class="bg-black rounded sm:mt-10">
            <h2 class="text-center py-4 font-extrabold text-gray-200">
              LIVE PREVIEW
            </h2>
            <div id="browserFrame" class="overflow-hidden flex flex-col">
              <div
                id="topBar"
                class="
                  topbar
                  border-r-4 border-l-4 border-black
                  bg-gray-900
                  z-10
                "
              >
                <div id="searchField" class="p-2 flex items-center">
                  <input
                    type="text"
                    class="pl-4 h-12 w-full bg-black rounded text-gray-500"
                    aria-label="vCard URL"
                    disabled
                    :value="'https://yoursite/vcard/' + username"
                    tabindex="-1"
                  />
                  <div
                    class="w-6 ml-2"
                    v-html="require(`~/assets/icons/ellipsis.svg?include`)"
                  ></div>
                </div>
              </div>
              <Preview
                class="rounded-b-2xl"
                ref="html"
                :username="username"
                :genInfo="genInfo"
                :images="images"
                :featured="featured"
                :colors="colors"
                :primaryActions="allActions"
                :secondaryActions="secondaryActions"
                :PreviewMode="PreviewMode"
                :downloadVcard="downloadVcard"
                :footerCredit="footerCredit"
                :showAlert="showAlert"
                :hasLightBG="hasLightBG"
                :downloadKey="downloadKey"
                :pubKeyIsValid="pubKeyIsValid"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
    <Vcard ref="vCard" :vCard="vCard" />
    <Footer />
  </div>
</template>

<script>
import Modal from '@/components/Modal'
import Attachment from '@/components/Attachment'
import Action from '@/components/Action'
import Featured from '@/components/Featured'
import Colour from '@/components/Colour'
import Preview from '@/components/Preview'
import Download from '@/components/Download'
import Help from '@/components/Help'
import Footer from '@/components/Footer'
import Cropper from '@/components/Cropper'

import Vcard from '@/components/Vcard'
import JSZip from 'jszip'
import draggable from 'vuedraggable'

import { saveAs } from 'file-saver'
import QRCode from '!!raw-loader!~/static/qrcode.min.js'
import Theme1 from '!!raw-loader!~/assets/styles/T1.min.css'
import Theme2 from '!!raw-loader!~/assets/styles/T2.min.css'
import Theme3 from '!!raw-loader!~/assets/styles/T3.min.css'
import { mapState, mapActions } from 'vuex'

export default {
  components: {
    Cropper,
    Modal,
    Attachment,
    Action,
    Featured,
    Colour,
    Preview,
    Download,
    Help,
    Footer,
    Vcard,
    draggable,
  },

  data() {
    return {
      downloadCheckList: [
        {
          label:
            'I did not attach any link or file that will cause any risk to the user',
          checked: false,
        },
        {
          label: 'I have verified that all the links are working correctly',
          checked: false,
        },
        {
          label: 'I have removed all unused fields and sections',
          checked: false,
        },
      ],
      images: {
        logo: {
          url: null,
          blob: null,
          ext: null,
          mime: null,
          resized: null,
        },
        photo: {
          url: null,
          blob: null,
          ext: null,
          mime: null,
          resized: null,
        },
        cover: {
          url: null,
          blob: null,
          ext: null,
          mime: null,
          resized: null,
        },
      },
      colors: {
        logoBg: {
          color: `#2c2c2c`,
          openPalette: false,
        },
        mainBg: {
          color: `#ffffff`,
          openPalette: false,
        },
        buttonBg: {
          color: `ffffff`,
          openPalette: false,
        },
        cardBg: {
          color: `#ffffff`,
          openPalette: false,
        },
      },
      genInfo: {
        fname: null,
        lname: null,
        pronouns: null,
        title: null,
        biz: 'DB&B Philippines, Inc.',
        addr: '26F Alveo Financial Tower\n6794 Ayala Ave Makati 1226',
        desc: 'Singapore | China | Philippines | Thailand',
        key: null,
        tracker: null,
        fontLink: '<link rel="preconnect" href="https://fonts.googleapis.com"><link rel="preconnect" href="https://fonts.gstatic.com" crossorigin><link href="https://fonts.googleapis.com/css2?family=Jost:ital,wght@0,100..900;1,100..900&display=swap" rel="stylesheet">',
        fontCss: 'font-family: "Jost", sans-serif; font-optical-sizing: auto; font-weight: 400; font-style: normal;',
      },
      primaryActions: [
          {
            name: 'Mobile',
            icon: 'call',
            href: 'tel:',
            placeholder: '+63 900 000 0000',
            value: '+63 900 000 0000',
            label: 'Mobile number',
            order: 0,
            isURL: 0,
          },
          {
            name: 'Email',
            icon: 'email',
            href: 'mailto:',
            placeholder: 'name@dbb.com.ph',
            value: 'name@dbb.com.ph',
            label: 'Email address',
            order: 4,
          },
      ],
      filterPrimary: '',
      secondaryActions: [],
      filterSecondary: '',
      actions: {
        primaryActions: [
          {
            name: 'Viber',
            icon: 'viber',
            href: 'viber://chat?number=',
            placeholder: 'XX XXXXX XXXXX',
            value: null,
            label: 'Viber mobile number',
            order: 15,
            isURL: 1,
          },
          {
            name: 'Signal',
            icon: 'signal',
            href: 'https://signal.me/#p/',
            placeholder: '+XXXXXXXXXXXX',
            value: null,
            label: 'Signal number with country code (no spaces)',
            order: 8,
            isURL: 1,
          },
          {
            name: 'Telegram',
            icon: 'telegram',
            href: 'https://t.me/',
            placeholder: 'username',
            value: null,
            label: 'Telegram username',
            order: 9,
            isURL: 1,
          },
          {
            name: 'WhatsApp',
            icon: 'whatsapp',
            placeholder: 'https://wa.me/profileID',
            value: null,
            label: 'WhatsApp profile URL',
            order: 11,
            isURL: 1,
          },
          {
            name: 'Messenger',
            icon: 'messenger',
            href: 'https://m.me/',
            placeholder: 'username',
            value: null,
            label: 'Messenger username',
            order: 12,
            isURL: 1,
          },
          {
            name: 'Line',
            icon: 'line',
            href: 'https://line.me/ti/p/',
            placeholder: 'LINE ID',
            value: null,
            label: 'Line profile ID',
            order: 14,
            isURL: 1,
          },
          {
            name: 'WeChat',
            icon: 'wechat',
            href: 'weixin://dl/chat?',
            placeholder: 'WeChat ID',
            value: null,
            label: 'WeChat profile ID',
            order: 16,
            isURL: 1,
          },
          {
            name: 'Calendar',
            icon: 'calendar',
            placeholder: 'https://example.com/calendarID',
            value: null,
            label: 'Calendar URL',
            order: 17,
            isURL: 1,
          },
        ],
        secondaryActions: [], 
      },
      featured: [
        {
          title: null,
          content: [],
        },
      ],
      hostedURL: null,
      PreviewMode: true,
      content: null,
      inView: false,
      showPreview: false,
      scrollPos: null,
      opening: false,
    }
  },
  computed: {
    ...mapState(['theme']),
    getFullname() {
      let fn = this.genInfo.fname
      let ln = this.genInfo.lname
      return (fn + ln).length ? `${fn ? fn : ''}${ln ? ' ' + ln : ''}` : null
    },
    pubKeyIsValid() {
      return false
    },
    downloadChecked() {
      return this.downloadCheckList.filter((e) => e.checked).length == 3
    },
    username() {
      return this.getFullname
        ? this.getFullname.toLowerCase().replace(/\W+/g, '')
        : 'username'
    },
    orderedPrimaryActions() {
      return this.actions.primaryActions.sort((a, b) =>
        a.order > b.order ? 1 : a.order < b.order ? -1 : 0
      )
    },
    filteredPrimaryActions() {
      return this.orderedPrimaryActions.filter((e) =>
        e.name.toLowerCase().includes(this.filterPrimary.toLowerCase())
      )
    },
    orderedSecondaryActions() {
      return this.actions.secondaryActions.sort((a, b) =>
        a.name.localeCompare(b.name)
      )
    },
    filteredSecondaryActions() {
      return this.orderedSecondaryActions.filter((e) =>
        e.name.toLowerCase().includes(this.filterSecondary.toLowerCase())
      )
    },
    // COMBINED ACTIONS: Fixed Actions appear BEFORE Editable Actions
    allActions() {
      let editable = this.primaryActions;
      let fixed = [
        {
          name: 'Office',
          icon: 'call',
          value: '+63 2 7745 8800',
          href: 'tel:',
          isURL: 0,
        },
        {
          name: 'Website',
          icon: 'website',
          value: 'https://www.dbb.com',
          isURL: 1,
        },
        {
          name: 'Location',
          icon: 'location',
          value: 'https://maps.app.goo.gl/9JPriTcXZixMACVn9',
          isURL: 1,
        }
      ];
      // Spread fixed first, then editable
      return [...fixed, ...editable];
    },
    vCard() {
      const getNumber = (type) => {
        let no = this.allActions
          .map((e) => (e.name == type ? e.value : null))
          .filter((e) => e)[0]
        return no ? no.replace(/\s/g, '') : null
      }
      let email = this.allActions
        .map((e) => (e.name == 'Email' ? e.value : null))
        .filter((e) => e)[0]
      
      let actions = [
        ...this.allActions,
        ...this.secondaryActions.map((e) => {
          return { ...e, isURL: 1 }
        }),
      ]
      let urls = actions
        .map((e) => {
          if (e.isURL && e.value) {
            return {
              title: e.name,
              url:
                (e.href ? e.href : '') + e.value + (e.hrefEnd ? e.hrefEnd : ''),
            }
          }
          return false
        })
        .filter((e) => e)

      let note = this.genInfo.desc
        ? this.genInfo.desc.replace(/[\r\n]+/gm, '')
        : null
      let key = null
      let randomNumber = Math.floor(100000000 + Math.random() * 900000)
      return {
        fn: this.genInfo.fname,
        ln: this.genInfo.lname,
        title: this.genInfo.title,
        org: this.genInfo.biz,
        addr: this.genInfo.addr,
        cell: getNumber('Mobile'),
        work: getNumber('Office'),
        home: getNumber('Home'),
        sms: getNumber('SMS'),
        email,
        hostedURL: this.hostedURL,
        website: 'https://www.dbb.com',
        urls,
        key,
        note,
        uid: `EnBizCard-${randomNumber}`,
      }
    },
  },
  methods: {
    ...mapActions(['changeTheme']),
    togglePreview() {
      this.opening = true
      let c = this.$refs.container
      if (this.showPreview) {
        c.classList.remove('overflow-y-hidden', 'h-screen')
        window.scrollTo(0, this.scrollPos)
        this.opening = false
      } else {
        this.scrollPos = window.scrollY
        setTimeout(() => {
          c.classList.add('overflow-y-hidden', 'h-screen')
          this.opening = false
        }, 400)
      }
      this.showPreview = !this.showPreview
    },
    checkView() {
      let e = this.$refs.create
      if (e) {
        let top = e.getBoundingClientRect().top
        this.inView = this.showPreview ? true : top < 0
      }
    },
    clearContent() {
      this.content = null
    },
    create() {
      this.$refs.create.scrollIntoView({ behavior: 'smooth' })
    },
    getTitle(e) {
      return e.toLowerCase().split(' ').join('_')
    },
    addFeature() {
      this.featured.push({
        title: 'Section title',
        content: [],
      })
    },
    hasLightBG(e) {
      let hex = this.colors[e].color
      hex = hex.slice(1)
      if (hex.length === 3) {
        hex = hex[0] + hex[0] + hex[1] + hex[1] + hex[2] + hex[2]
      }
      let r = parseInt(hex.slice(0, 2), 16)
      let g = parseInt(hex.slice(2, 4), 16)
      let b = parseInt(hex.slice(4, 6), 16)
      const brightness = Math.round(
        (parseInt(r) * 299 + parseInt(g) * 587 + parseInt(b) * 114) / 1000
      )
      return brightness > 125 ? true : false
    },
    showAlert(content) {
      this.content = content
    },
    clearFilterActions() {
      this.filterPrimary = this.filterSecondary = ''
    },
    filteredAction(filterType, actionType) {
      if (this[filterType].length)
        this.addAction(actionType, this[filterType][0].name)
      this.clearFilterActions()
    },
    addAction(type, name) {
      let index = this.actions[type].findIndex((e) => e.name === name)
      this[type].push(this.actions[type][index])
      this.actions[type].splice(index, 1)
      this.clearFilterActions()
    },
    removeAction(type, index) {
      this.actions[type].unshift(this[type][index])
      this[type].splice(index, 1)
    },
    downloadVcard() {
      let blob = new Blob([this.$refs.vCard.$refs.vCard.innerText], {
        type: 'text/plain',
      })
      saveAs(window.URL.createObjectURL(blob), `${this.username}.vcf`)
    },
    downloadKey() {
      // Disabled functionality
    },
    async resizeImage(type, mime, index1, index2) {
      let vm = this
      let reader = new FileReader()
      let file
      if (index2 >= 0) {
        if (type == 'image') {
          file = await this.featured[index1].content[index2].file
        } else if (type == 'music') {
          file = await this.featured[index1].content[index2].cover
        } else if (type == 'product') {
          file = await this.featured[index1].content[index2].image.file
        }
      } else {
        file = await this.images[type].blob
      }
      let canvas = document.createElement('canvas')
      let ctx = canvas.getContext('2d')
      let img = document.createElement('img')
      let maxWidth, maxHeight
      reader.onload = (e) => {
        img.src = e.target.result
        img.onload = () => {
          if (type == 'photo') {
            canvas.width = canvas.height = 320
          } else {
            if (type == 'logo') {
              maxWidth = 960
              maxHeight = 192
            } else {
              maxWidth = maxHeight = 960
            }
            let width = img.width
            let height = img.height

            if (width > maxWidth) {
              height *= maxWidth / width
              width = maxWidth
            }
            if (height > maxHeight) {
              width *= maxHeight / height
              height = maxHeight
            }
            canvas.width = width
            canvas.height = height
          }
          ctx.drawImage(img, 0, 0, canvas.width, canvas.height)
          canvas.toBlob(
            (blob) => {
              let image = new File([blob], type, {
                type: mime,
              })
              if (index2 >= 0) {
                if (type == 'image') {
                  vm.featured[index1].content[index2].file = image
                } else if (type == 'music') {
                  vm.featured[index1].content[index2].cover = image
                } else if (type == 'product') {
                  vm.featured[index1].content[index2].image.file = image
                }
              } else {
                vm.images[type].resized = image
              }
            },
            mime,
            0.8
          )
        }
      }
      reader.readAsDataURL(file)
    },
    getTrackingCode() {
      return false
    },
    downloadPackage() {
      if (this.downloadChecked) {
        this.PreviewMode = false
        setTimeout(() => {
          let el = new DOMParser().parseFromString(
            this.$refs.html.$refs.html.outerHTML,
            'text/html'
          )

          // Inject stylesheets
          let styleLink = document.createElement('link')
          styleLink.rel = 'stylesheet'
          styleLink.href = './style.min.css'
          el.querySelector('head').appendChild(styleLink)

          // Inject qrcode script
          let qrcode = document.createElement('script')
          qrcode.src = './qrcode.min.js'
          el.querySelector('body').appendChild(qrcode)

          // Inject general script
          let modals = document.createElement('script')
          modals.innerText =
            'let m=document.getElementById("modal"),c=document.getElementById("close"),ki=document.getElementById("keyView"),cv=document.getElementById("copyView"),curl=document.getElementById("copyURL"),qrv=document.getElementById("qrView"),qr=document.getElementById("qr"),s=document.getElementById("share"),sqr=document.getElementById("showQR"),sk=document.getElementById("showKey");function tC(e){"2rem"==e.style.top?(e.style.visibility="visible",e.style.top="0px",e.style.opacity=1):(e.style.top="2rem",e.style.opacity=0,setTimeout(()=>{e.style.visibility="hidden"},200))}function dN(e){e.style.display="none"}window.addEventListener("load",()=>{document.querySelector("#topActions").style.display="flex",qr.innerHTML=new QRCode({content:window.location.href,container:"svg-viewbox",join:!0,ecl:"L",padding:0}).svg()}),navigator.canShare?s.addEventListener("click",()=>{navigator.share({title:document.title,text:"You can view my Digital Business Card here:",url:window.location.href})}):s.addEventListener("click",()=>{tC(m),cv.style.display="flex",dN(qrv),ki&&dN(ki)}),sqr.addEventListener("click",()=>{tC(m),qrv.style.display="block",dN(cv),ki&&dN(ki)}),sk&&sk.addEventListener("click",()=>{tC(m),ki&&(ki.style.display="flex"),dN(cv),dN(qrv)}),c.addEventListener("click",()=>tC(m)),curl.addEventListener("click",async()=>{let e=curl.querySelectorAll(".iconColor")[1];await navigator.clipboard.writeText(window.location.href).then(t=>{e.innerText="Copied",setTimeout(()=>{e.innerText="Copy URL"},1e3)})});'
          el.querySelector('body').appendChild(modals)

          // Inject media script
          let mediaHandler = document.createElement('script')
          mediaHandler.innerText =
            'let pC=document.querySelectorAll(".pCtrl"),pP=document.querySelectorAll(".playPause"),srcs=document.querySelectorAll(".source");srcs.forEach(e=>{e.style.pointerEvents="none",e.removeAttribute("controls")}),pC.forEach((e,l)=>{e.style.display="flex";let r=e.querySelector(".currentTime"),s=e.querySelector(".seekBar"),t=e.querySelector(".playPause"),a=t.querySelector(".play"),c=t.querySelector(".pause");srcs[l].addEventListener("timeupdate",()=>{let e=srcs[l].currentTime,t=100/srcs[l].duration*e;s.value=t,100==t&&(s.value=0,a.style.display="block",c.style.display="none");let o=Math.floor(e/60),y=Math.floor(e%60);o.toString().length<2&&(o="0"+o),y.toString().length<2&&(y="0"+y),r.value=o+":"+y}),s.addEventListener("change",()=>{srcs[l].currentTime=srcs[l].duration*(parseInt(s.value)/100)}),t.addEventListener("click",()=>{srcs[l].paused?(srcs.forEach((e,r)=>{l!=r&&(e.paused||e.pause())}),pP.forEach((e,l)=>{let r=e.querySelector(".play"),s=e.querySelector(".pause");r.style.display="block",s.style.display="none"}),srcs[l].play(),a.style.display="none",c.style.display="block"):(srcs[l].pause(),c.style.display="none",a.style.display="block")})});'
          if (this.featured.length)
            el.querySelector('body').appendChild(mediaHandler)

          // Create blobs
          let html = new Blob(
            [`<!DOCTYPE html>${el.documentElement.outerHTML}`],
            {
              type: 'text/html',
            }
          )
          let theme = 1
          switch (this.theme) {
            case 1:
              theme = Theme1
              break
            case 2:
              theme = Theme2
              break
            case 3:
              theme = Theme3
              break
          }
          let css = new Blob([theme], {
            type: 'text/css',
          })
          let vCard = new Blob([this.$refs.vCard.$refs.vCard.innerText], {
            type: 'text/plain',
          })
          let guide = new Blob(
            [
              '<html><head><meta http-equiv="refresh" content="0; url=https://enbizcard.vishnuraghav.com/hosting-guide" /></head></html>',
            ],
            {
              type: 'text/html',
            }
          )
          let qrScript = new Blob([QRCode], {
            type: 'application/javascript',
          })

          // Prepare files
          let username = this.username
          let zip = new JSZip()
          zip.folder(username).file('index.html', html)
          zip.folder(username).file('style.min.css', css)
          zip.folder(username).file('qrcode.min.js', qrScript)
          zip.file('Hosting-Guide.html', guide)

          // Image attachments
          for (const key in this.images) {
            if (this.images[key].url) {
              zip
                .folder(username)
                .file(
                  `${key}.${this.images[key].ext}`,
                  this.images[key].resized
                )
            }
          }

          // Featured content
          let hasFeaturedContent = this.featured.filter(
            (e) => e.content.length
          ).length
          if (hasFeaturedContent) {
            this.featured.forEach((item) => {
              item.content.forEach((item) => {
                if (item.contentType == 'media') {
                  zip
                    .folder(username)
                    .folder('media')
                    .file(`${this.getTitle(item.title)}.${item.ext}`, item.file)
                  if (item.type.match(/music|document/gi)) {
                    if (!item.info) {
                      zip
                        .folder(username)
                        .folder('media')
                        .file(
                          `${this.getTitle(item.title)}.${item.coverExt}`,
                          item.cover
                        )
                    }
                  }
                } else if (item.contentType == 'product' && item.image) {
                  zip
                    .folder(username)
                    .folder('media')
                    .file(
                      `${this.getTitle(item.image.title)}.${item.image.ext}`,
                      item.image.file
                    )
                }
              })
            })
          }

          // VCARD
          zip.folder(username).file(`${username}.vcf`, vCard)

          // Final ZIP file
          zip
            .generateAsync({
              type: 'blob',
            })
            .then(function (zip) {
              saveAs(zip, `${username}'s Digital Business Card.zip`)
            })
          this.PreviewMode = true
        }, 250)
      }
    },
  },
  mounted() {
    window.addEventListener('scroll', this.checkView)
    // window.onbeforeunload = function () {
    //   return 'Your work will be lost.'
    // }
  },
}
</script>
