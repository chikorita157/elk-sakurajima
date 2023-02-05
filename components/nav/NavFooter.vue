<script setup lang="ts">
const buildInfo = useAppConfig().buildInfo
const timeAgoOptions = useTimeAgoOptions()

const userSettings = useUserSettings()

const buildTimeDate = new Date(buildInfo.time)
const buildTimeAgo = useTimeAgo(buildTimeDate, timeAgoOptions)

const colorMode = useColorMode()
function toggleDark() {
  colorMode.preference = colorMode.value === 'dark' ? 'light' : 'dark'
}
</script>

<template>
  <footer p4 text-sm text-secondary-light flex="~ col">
    <div flex="~ gap2" items-center mb4>
      <CommonTooltip :content="$t('nav.toggle_theme')">
        <button flex i-ri:sun-line dark-i-ri:moon-line text-lg :aria-label="$t('nav.toggle_theme')" @click="toggleDark()" />
      </CommonTooltip>
      <CommonTooltip :content="$t('nav.zen_mode')">
        <button
          flex
          text-lg
          :class="userSettings.zenMode ? 'i-ri:layout-right-2-line' : 'i-ri:layout-right-line'"
          :aria-label="$t('nav.zen_mode')"
          @click="userSettings.zenMode = !userSettings.zenMode"
        />
      </CommonTooltip>
      <CommonTooltip :content="$t('settings.about.sponsor_action')">
        <NuxtLink
          flex
          text-lg
          i-ri-heart-3-line hover="i-ri-heart-3-fill text-rose"
          :aria-label="$t('settings.about.sponsor_action')"
          href="https://github.com/sponsors/elk-zone"
          target="_blank"
        />
      </CommonTooltip>
    </div>
    <div>
      <i18n-t v-if="isHydrated" keypath="nav.built_at">
        <time :datetime="String(buildTimeDate)" :title="$d(buildTimeDate, 'long')">{{ buildTimeAgo }}</time>
      </i18n-t>
      <span v-else>
        {{ $t('nav.built_at', [$d(buildTimeDate, 'shortDate')]) }}
      </span>
      &middot;
      <NuxtLink
        v-if="buildInfo.env === 'release'"
        external
        :href="`https://github.com/chikorita157/elk-sakurajima/releases/tag/v${buildInfo.version}`"
        target="_blank"
        font-mono
      >
        v{{ buildInfo.version }}
      </NuxtLink>
      <span v-else>{{ buildInfo.env }}</span>
      <template v-if="buildInfo.commit && buildInfo.branch !== 'release'">
        &middot;
        <NuxtLink
          external
          :href="`https://github.com/chikorita157/elk-sakurajima/commit/${buildInfo.commit}`"
          target="_blank"
          font-mono
        >
          {{ buildInfo.commit.slice(0, 7) }}
        </NuxtLink>
      </template>
    </div>
    <div>
      <NuxtLink cursor-pointer hover:underline to="/settings/about">
        {{ $t('settings.about.label') }}
      </NuxtLink>
      <template v-if="$config.public.privacyPolicyUrl">
        &middot;
        <NuxtLink cursor-pointer hover:underline :to="$config.public.privacyPolicyUrl">
          {{ $t('nav.privacy') }}
        </NuxtLink>
      </template>
      &middot;
      <NuxtLink href="/sakurajima.moe/@sakurajima" target="_blank">
        Mastodon
      </NuxtLink>
      &middot;
      <NuxtLink href="https://blog.sakurajima.moe" target="_blank" external>
        Blog
      </NuxtLink>
      &middot;
      <NuxtLink href="https://usuzakuraya.us" target="_blank" external>
       Pixelfed
      </NuxtLink>
      &middot;
      <NuxtLink href="https://forums.sakurajima.moe" target="_blank" external>
        Forums
      </NuxtLink>
      &middot;
      <NuxtLink href="https://www.patreon.com/sakurajimamastodon" target="_blank" external>
        Patreon
      </NuxtLink>
      &middot;
      <NuxtLink href="https://ko-fi.com/V7V8GAJR9" target="_blank" external>
        Ko-fi
      </NuxtLink>
      &middot;
      <NuxtLink href="https://www.paypal.com/donate/?hosted_button_id=HREN4ATRLZ54S" target="_blank" external>
        Paypal
      </NuxtLink>
      &middot;
      <NuxtLink href="https://github.com/chikorita157/elk-sakurajima" target="_blank" external>
        GitHub
      </NuxtLink>
    </div>
  </footer>
</template>
