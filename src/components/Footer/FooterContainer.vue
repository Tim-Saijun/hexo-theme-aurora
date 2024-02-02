<template>
  <div id="footer" class="relative w-full pt-0" :style="gradientBackground">
    <span class="bg-ob-deep-800 flex justify-center">
      <div
        class="bg-ob-deep-800 rounded-lg max-w-10/12 lg:max-w-screen-2xl text-sm text-ob-normal w-full py-6 px-6 grid grid-rows-1 lg:grid-rows-none lg:grid-cols-4 justify-center items-center gap-8"
      >
        <div
          class="flex flex-col lg:flex-row gap-6 lg:gap-12 row-span-1 lg:col-span-3 text-center lg:text-left"
        >
          <ul class="flex flex-col gap-1.5">
            <li
              v-if="
                themeConfig.site.beian.number !== '' ||
                themeConfig.site.police_beian.number !== ''
              "
              class="flex flex-row gap-3"
            >
              <span v-if="themeConfig.site.police_beian.number !== ''">
                <img class="inline-block" :src="beianImg" alt="" width="15" />
                <b>
                  <a :href="themeConfig.site.police_beian.link">
                    <b
                      class="font-extrabold border-b-2 border-ob hover:text-ob"
                    >
                      {{ themeConfig.site.police_beian.number }}
                    </b>
                  </a>
                </b>
              </span>
              <span v-if="themeConfig.site.beian.number !== ''">
                <a :href="themeConfig.site.beian.link">
                  <b class="font-extrabold border-b-2 border-ob hover:text-ob">
                    {{ themeConfig.site.beian.number }}
                  </b>
                </a>
              </span>
            </li>
            <li>
              萌ICP备
              <a href="https://icp.gov.moe/?keyword=20239566">
                <b class="font-extrabold border-b-2 border-ob hover:text-ob">
                20239566号
                </b>
              </a>
            </li>
            <li>
              Powered by
              <a href="https://hexo.io/">
                <b class="font-extrabold border-b-2 border-ob hover:text-ob">
                  Hexo
                </b>
              </a>
              & Themed by
              <a href="https://github.com/Tim-Saijun/hexo-theme-aurora-s">
                <b class="font-extrabold border-b-2 border-ob hover:text-ob">
                  Aurora-s v{{ themeConfig.version }}
                </b>
              </a>
            </li>
            <li>
              Copyright © 2022 - {{ currentYear }}
              <b class="font-extrabold">{{ themeConfig.site.author }}</b>
              . All Rights Reserved.
            </li>
          </ul>
          <ul class="flex flex-col flex-1 gap-1.5">
            <li
              class="flex flex-row max-w-[11rem]"
              v-if="enabledPlugin === 'waline'"
            >
              <span>
                <SvgIcon
                  icon-class="hot"
                  class="mr-1 text-lg inline-block"
                  stroke="currentColor"
                />
                {{ t('settings.page-views-value') }}
              </span>
              <span class="flex-1 text-right">
                <span class="waline-pageview-count" data-path="/" />
              </span>
            </li>

            <li
              class="flex flex-row max-w-[11rem]"
              v-if="themeConfig.plugins.busuanzi.enable"
            >
              <span>
                <SvgIcon
                  icon-class="hot"
                  class="mr-1 text-lg inline-block"
                  stroke="currentColor"
                />
                {{ t('settings.page-views-value') }}
              </span>
              <span class="flex-1 text-right" id="busuanzi_container_site_pv">
                <span id="busuanzi_value_site_pv"
              /></span>
            </li>

            <li
              class="flex flex-row max-w-[11rem]"
              v-if="themeConfig.plugins.busuanzi.enable"
            >
              <span>
                <SvgIcon
                  icon-class="friends"
                  class="mr-1 text-lg inline-block"
                  stroke="currentColor"
                />
                {{ t('settings.unique_visitor-value') }}
              </span>
              <span id="busuanzi_container_site_uv" class="flex-1 text-right">
                <span id="busuanzi_value_site_uv"
              /></span>
            </li>

            <li v-if="runningDays" class="flex flex-row max-w-[11rem]">
              <span>
                <SvgIcon
                  icon-class="date"
                  class="mr-1 text-lg inline-block"
                  stroke="currentColor"
                />
                运行天数：{{runningDays}}
                <!-- {{ t('settings.site-running-for') }} -->
              </span>
              <!-- <span class="flex-1 text-right"
                >{{ runningDays }}
                {{ t('settings.site-running-for-unit') }}</span
              > -->
            </li>
          </ul>
        </div>
        <div
          class="hidden lg:flex lg:col-span-1 justify-center lg:justify-end row-span-1 relative"
        >
          <!-- <img
            v-show="themeConfig.site.avatar"
            :class="avatarClass"
            :src="themeConfig.site.avatar"
            alt="avatar"
          /> -->
          <a href="https://www.upyun.com/?utm_source=lianmeng&utm_medium=referral" target="_blank" alt="又拍云" rel="nofollow">
            本站由<img data-v-a478af14="" src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxMTEiIGhlaWdodD0iMzgiIHZpZXdCb3g9IjAgMCAxMTEgMzgiPgogIDxnIGZpbGw9IiNGRkZGRkYiIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0xKSI+CiAgICA8cGF0aCBkPSJNMzAuOCw0LjEgTDMwLjgsNC4xIEwzMC44LDQuMSBMMzAuOCw0LjEgTDMwLjgsNC4xIEMzMC4xLDMuNiAyOS40LDMuMSAyOC42LDIuNyBDMjguMSwyLjQgMjcuNCwyLjUgMjcsMyBMMjIuMyw5LjIgTDIyLDkuNiBDMjEuNSwxMC4zIDIwLjcsMTAuNiAxOS45LDEwLjYgTDE5LDEwLjYgQzE2LjcsMTAuNyAxNC40LDExLjkgMTIuOSwxMy45IEMxMS43LDE1LjUgMTEuMSwxNy40IDExLjIsMTkuMyBDMTEuMiwxOS42IDExLjQsMTkuOSAxMS43LDIwLjEgQzEyLjMsMjAuNCAxMi44LDIxIDEzLDIxLjcgQzEzLjIsMjIuOCAxMi41LDIzLjggMTEuNSwyNC4xIEMxMC4zLDI0LjUgOSwyMy43IDguNywyMi41IEM4LjUsMjEuOCA4LjcsMjEuMiA5LjEsMjAuNyBDOS4zLDIwLjQgOS40LDIwLjEgOS40LDE5LjcgQzkuMiwxNy4zIDkuOCwxNC44IDExLjQsMTIuNyBDMTMuNCwxMC4xIDE2LjQsOC43IDE5LjUsOC43IEMyMC4xLDguNyAyMC42LDguNCAyMSw3LjkgTDI1LjMsMi4yIEMyNS42LDEuOCAyNS40LDEuMSAyNC45LDEgQzE3LjcsLTEgOS43LDEuNCA0LjgsNy43IEMtMS40LDE1LjggMC4xLDI3LjUgOC4yLDMzLjggQzksMzQuNCA5LjcsMzQuOSAxMC42LDM1LjQgQzExLjEsMzUuNyAxMS44LDM1LjYgMTIuMiwzNS4xIEwxNi45LDI4LjkgTDE3LjIsMjguNSBDMTcuNywyNy44IDE4LjUsMjcuNSAxOS4zLDI3LjUgTDIwLjIsMjcuNSBDMjIuNSwyNy40IDI0LjgsMjYuMiAyNi4zLDI0LjIgQzI3LjUsMjIuNiAyOC4xLDIwLjcgMjgsMTguOCBDMjgsMTguNSAyNy44LDE4LjIgMjcuNSwxOCBDMjYuOSwxNy43IDI2LjQsMTcuMSAyNi4yLDE2LjQgQzI2LDE1LjMgMjYuNywxNC4zIDI3LjcsMTQgQzI4LjksMTMuNiAzMC4yLDE0LjQgMzAuNSwxNS42IEMzMC43LDE2LjMgMzAuNSwxNi45IDMwLjEsMTcuNCBDMjkuOSwxNy43IDI5LjgsMTggMjkuOCwxOC40IEMzMCwyMC44IDI5LjQsMjMuMyAyNy44LDI1LjQgQzI1LjgsMjggMjIuOCwyOS40IDE5LjcsMjkuNCBDMTkuMSwyOS40IDE4LjYsMjkuNyAxOC4yLDMwLjIgTDE0LDM1LjYgQzEzLjcsMzYgMTMuOSwzNi43IDE0LjQsMzYuOCBDMjEuNiwzOC45IDI5LjcsMzYuNSAzNC41LDMwLjEgQzQwLjcsMjIgMzksMTAuMyAzMC44LDQuMSBaIi8+CiAgICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSg0MyA4KSI+CiAgICAgIDxwYXRoIGQ9Ik0xOC42LDIgTDE4LjYsMS42IEMxOC42LDEuMiAxOC4zLDEgMTgsMSBMMy41LDEgQzMuMiwxIDIuOSwxLjMgMi45LDEuNiBMMi45LDIuNSBDMi45LDIuOCAzLjIsMy4xIDMuNSwzLjEgTDE1LjgsMy4xIEMxNi4yLDMuMSAxNi40LDMuNCAxNi40LDMuOCBDMTYuMSw2LjEgMTUsMTAuNCAxMS42LDE0LjUgQzExLjQsMTQuOCAxMC45LDE0LjggMTAuNywxNC41IEM3LjksMTAuOSA3LDcuMiA2LjgsNS44IEM2LjcsNS41IDYuNSw1LjMgNi4yLDUuMyBMNS43LDUuMyBMNS4zLDUuMyBDNSw1LjMgNC43LDUuNiA0LjgsNiBDNS4xLDcuNiA2LjEsMTEuOSA5LjUsMTYgQzkuNywxNi4yIDkuNywxNi42IDkuNSwxNi44IEM1LjUsMjAuNSAxLjgsMjEuMyAwLjUsMjEuNSBDMC4yLDIxLjUgNy4xMDU0MjczNmUtMTUsMjEuOCA3LjEwNTQyNzM2ZS0xNSwyMi4xIEw3LjEwNTQyNzM2ZS0xNSwyMyBDNy4xMDU0MjczNmUtMTUsMjMuMyAwLjMsMjMuNiAwLjYsMjMuNiBDMi4xLDIzLjQgNi4zLDIyLjUgMTAuOCwxOC4zIEMxMSwxOC4xIDExLjQsMTguMSAxMS42LDE4LjMgQzEzLjgsMjAuMyAxNi42LDIyLjMgMjAuNCwyMy41IEMyMC43LDIzLjYgMjEsMjMuNCAyMS4xLDIzLjEgTDIxLjQsMjIuMyBDMjEuNSwyMiAyMS4zLDIxLjcgMjEsMjEuNiBDMTcuNSwyMC41IDE1LDE4LjcgMTMsMTYuOSBDMTIuOCwxNi43IDEyLjgsMTYuMyAxMywxNi4xIEMxOC41LDkuNCAxOC42LDIuMyAxOC42LDIgWiIvPgogICAgICA8cGF0aCBkPSJNMjguNiwwLjEgTDI3LjcsMC4xIEMyNy40LDAuMSAyNy4xLDAuNCAyNy4xLDAuNyBMMjcuMSwzLjggQzI3LjEsNC4xIDI2LjgsNC40IDI2LjUsNC40IEwyNC40LDQuNCBDMjQuMSw0LjQgMjMuOCw0LjcgMjMuOCw1IEwyMy44LDUuOSBDMjMuOCw2LjIgMjQuMSw2LjUgMjQuNCw2LjUgTDI2LjUsNi41IEMyNi44LDYuNSAyNy4xLDYuOCAyNy4xLDcuMSBMMjcuMSwxMS42IEMyNy4xLDExLjkgMjYuOCwxMi4yIDI2LjUsMTIuMiBMMjQuNCwxMi4yIEMyNC4xLDEyLjIgMjMuOCwxMi41IDIzLjgsMTIuOCBMMjMuOCwxMy43IEMyMy44LDE0IDI0LjEsMTQuMyAyNC40LDE0LjMgTDI2LjUsMTQuMyBDMjYuOCwxNC4zIDI3LjEsMTQuNiAyNy4xLDE0LjkgTDI3LjEsMTkuMiBDMjcuMSwyMC4xIDI2LjcsMjEuNiAyNC40LDIxLjggQzI0LjEsMjEuOCAyMy45LDIyLjEgMjMuOSwyMi40IEwyMy45LDIzIEMyMy45LDIzLjMgMjQuMiwyMy42IDI0LjUsMjMuNiBDMjYuOCwyMy40IDI5LjIsMjIuMSAyOS4yLDE4LjkgTDI5LjIsMTQuNiBDMjkuMiwxNC4zIDI5LjUsMTQgMjkuOCwxNCBMMzEuNywxNCBDMzIsMTQgMzIuMywxMy43IDMyLjMsMTMuNCBMMzIuMywxMi41IEMzMi4zLDEyLjIgMzIsMTEuOSAzMS43LDExLjkgTDI5LjgsMTEuOSBDMjkuNSwxMS45IDI5LjIsMTEuNiAyOS4yLDExLjMgTDI5LjIsNi44IEMyOS4yLDYuNSAyOS41LDYuMiAyOS44LDYuMiBMMzEuNyw2LjIgQzMyLDYuMiAzMi4zLDUuOSAzMi4zLDUuNiBMMzIuMyw0LjcgQzMyLjMsNC40IDMyLDQuMSAzMS43LDQuMSBMMjkuOCw0LjEgQzI5LjUsNC4xIDI5LjIsMy44IDI5LjIsMy41IEwyOS4yLDAuNiBDMjkuMiwwLjMgMjksMC4xIDI4LjYsMC4xIFoiLz4KICAgICAgPHBhdGggZD0iTTMzLjIsMjIuOCBDMzMuMiwyMy4xIDMzLjUsMjMuNCAzMy44LDIzLjQgTDQwLjMsMjMuNCBDNDMuMiwyMy40IDQ1LjUsMjEuMiA0NS41LDE4LjQgTDQ1LjUsNC43IEM0NS41LDQuNCA0NS4yLDQuMSA0NC45LDQuMSBMMzMuOCw0LjEgQzMzLjUsNC4xIDMzLjIsNC40IDMzLjIsNC43IEwzMy4yLDIyLjggWiBNNDAuNCwyMS40IEwzNS45LDIxLjQgQzM1LjYsMjEuNCAzNS4zLDIxLjEgMzUuMywyMC44IEwzNS4zLDE0LjYgQzM1LjMsMTQuMyAzNS42LDE0IDM1LjksMTQgTDQzLDE0IEM0My4zLDE0IDQzLjYsMTQuMyA0My42LDE0LjYgTDQzLjYsMTguNCBDNDMuNiwyMC4xIDQyLjEsMjEuNCA0MC40LDIxLjQgWiBNNDMuNiw2LjcgTDQzLjYsMTEuNCBDNDMuNiwxMS43IDQzLjMsMTIgNDMsMTIgTDM1LjksMTIgQzM1LjYsMTIgMzUuMywxMS43IDM1LjMsMTEuNCBMMzUuMyw2LjcgQzM1LjMsNi40IDM1LjYsNi4xIDM1LjksNi4xIEw0Myw2LjEgQzQzLjMsNi4yIDQzLjYsNi40IDQzLjYsNi43IFoiLz4KICAgICAgPHBhdGggZD0iTTQyLjEsMCBMMzYuMywwIEMzNiwwIDM1LjcsMC4zIDM1LjcsMC42IEwzNS43LDEuNSBDMzUuNywxLjggMzYsMi4xIDM2LjMsMi4xIEw0Mi4xLDIuMSBDNDIuNCwyLjEgNDIuNywxLjggNDIuNywxLjUgTDQyLjcsMC42IEM0Mi43LDAuMyA0Mi41LDAgNDIuMSwwIFoiLz4KICAgICAgPHBhdGggZD0iTTY0LjIsMC45IEw1Mi4yLDAuOSBDNTEuOSwwLjkgNTEuNiwxLjIgNTEuNiwxLjUgTDUxLjYsMi40IEM1MS42LDIuNyA1MS45LDMgNTIuMiwzIEw2NC4yLDMgQzY0LjUsMyA2NC44LDIuNyA2NC44LDIuNCBMNjQuOCwxLjUgQzY0LjgsMS4yIDY0LjUsMC45IDY0LjIsMC45IFoiLz4KICAgICAgPHBhdGggZD0iTTU5LjEsMTEuMiBMNjcuNSwxMS4yIEM2Ny44LDExLjIgNjguMSwxMC45IDY4LjEsMTAuNiBMNjguMSw5LjcgQzY4LjEsOS40IDY3LjgsOS4xIDY3LjUsOS4xIEw0OC45LDkuMSBDNDguNiw5LjEgNDguMyw5LjQgNDguMyw5LjcgTDQ4LjMsMTAuNiBDNDguMywxMC45IDQ4LjYsMTEuMiA0OC45LDExLjIgTDU0LjYsMTEuMiBDNTUuMSwxMS4yIDU1LjMsMTEuNyA1NS4xLDEyLjEgTDUxLjMsMTkgQzUxLjIsMTkuMiA1MS4xLDE5LjMgNTEuMSwxOS41IEM1MC44LDIwLjQgNTAuOSwyMS40IDUxLjUsMjIuMiBDNTIuMSwyMyA1My4xLDIzLjYgNTQuMiwyMy42IEw2NC42LDIzLjYgQzY1LjEsMjMuNiA2NS41LDIzLjQgNjUuNywyMyBDNjUuOSwyMi42IDY2LDIyLjEgNjUuOCwyMS43IEw2My40LDE3IEM2My4zLDE2LjcgNjIuOSwxNi42IDYyLjYsMTYuOCBMNjEuOCwxNy4yIEM2MS41LDE3LjMgNjEuNCwxNy43IDYxLjYsMTggTDYzLDIwLjggQzYzLjIsMjEuMiA2Mi45LDIxLjYgNjIuNSwyMS42IEw1NC4yLDIxLjYgQzUzLjgsMjEuNiA1My40LDIxLjQgNTMuMiwyMS4xIEM1My4xLDIxIDUyLjksMjAuNyA1MywyMC4zIEM1MywyMC4yIDUzLDIwLjIgNTMuMSwyMC4xIEw1Ny43LDEyIEM1OCwxMS41IDU4LjUsMTEuMiA1OS4xLDExLjIgWiIvPgogICAgPC9nPgogIDwvZz4KPC9zdmc+Cg==">提供CDN加速/云存储服务
          </a>
      </div>
      </div>
    </span>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, nextTick, watch } from 'vue'
import { useAppStore } from '@/stores/app'
import { useI18n } from 'vue-i18n'
import SvgIcon, { SvgTypes } from '@/components/SvgIcon/index.vue'
import beianImg from '@/assets/gongan-beian-40-40.png'
import { getDaysTillNow } from '@/utils'
import useCommentPlugin from '@/hooks/useCommentPlugin'

export default defineComponent({
  name: 'ObFooter',
  components: { SvgIcon },
  setup() {
    const appStore = useAppStore()
    const { t } = useI18n()
    const { enabledCommentPlugin, intiCommentPluginPageView } =
      useCommentPlugin()

    watch(
      () => appStore.configReady,
      async (newValue, oldValue) => {
        if (!oldValue && newValue) {
          await nextTick()
          intiCommentPluginPageView('/')
        }
      }
    )

    return {
      SvgTypes,
      beianImg,
      avatarClass: computed(() => {
        return {
          'footer-avatar': true,
          [appStore.themeConfig.theme.profile_shape]: true
        }
      }),
      gradientText: computed(
        () => appStore.themeConfig.theme.background_gradient_style
      ),
      gradientBackground: computed(() => {
        return { background: appStore.themeConfig.theme.header_gradient_css }
      }),
      currentYear: computed(() => new Date().getUTCFullYear()),
      themeConfig: computed(() => appStore.themeConfig),
      configReady: computed(() => appStore.configReady),
      runningDays: computed(() => {
        if (appStore.themeConfig.site.started_date === '') return undefined
        return getDaysTillNow(`${appStore.themeConfig.site.started_date}`)
      }),
      intiCommentPluginPageView,
      enabledPlugin: computed(() => enabledCommentPlugin.value.plugin),
      t
    }
  }
})
</script>

<style lang="scss" scoped></style>
