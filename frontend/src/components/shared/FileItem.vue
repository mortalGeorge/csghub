<template>
  <a :href="detailLink"
     :class="`${repoType}-card`"
     class="focus:outline focus:outline-4 focus:outline-gray-200 hover:shadow-md p-4 mlg:w-full border border-gray-200 rounded-lg flex-grow xl:basis-full min-w-[250px] xl:max-w-full"
     :style="isCollection ? 'width:100%' : ''"
  >
    <div class="flex items-center justify-between mb-1 w-[399px] lg:w-full">
      <div :class="`${repoType}-path`"
           class="text-md text-gray-700 font-normal text-ellipsis overflow-hidden whitespace-nowrap"
      >
        {{ file.path }}
      </div>
      <!-- <div class="flex gap-1">
        <el-tooltip
          effect="light"
          :content="$t('file.source.needSync')"
          placement="top"
        >
          <SvgIcon v-if="!!needSyncIcon" :name="needSyncIcon" />
        </el-tooltip>
        <el-tooltip
          effect="light"
          :content="syncTooltip"
          placement="top"
        >
          <SvgIcon v-if="!!sourceIcon && !needSyncIcon" :name="sourceIcon" />
        </el-tooltip>
      </div> -->
    </div>

    <p
      class="w-[390px] lg:w-[370px] mlg:w-full h-[35px] leading-[18px] mb-1 text-gray-500 text-sm overflow-hidden text-ellipsis line-clamp-2 text-left"
      :class="isCollection ? 'hidden' : ''"
     >
      {{ file.label }}
    </p>

    <div class="flex flex-nowrap overflow-hidden text-ellipsis items-center gap-[8px] text-xs text-gray-500">
      <!-- <span v-if="getComputed.taskTag"
            class="max-w-[80px] xl:max-w-full overflow-hidden text-ellipsis whitespace-nowrap"
      >
            {{ getComputed.taskTag }}
      </span>

      <span v-if="getComputed.taskTag">
        <SvgIcon name="vertical_divider" />
      </span> -->

      <div class="overflow-hidden text-ellipsis whitespace-nowrap">
        {{$t('repo.set.path')}}：{{ file['annotation_path'] }}
      </div>

      <!-- <template v-if="getComputed.visibility">
        <span> <SvgIcon name="vertical_divider" /> </span>
        <span class="visibility-label">{{ getComputed.visibility }}</span>
      </template>

      <span> <SvgIcon name="vertical_divider" /> </span>

      <span class="whitespace-nowrap">{{$t('all.downloadCount')}}：{{ file.downloads }}</span> -->
    </div>
  </a>
</template>

<script setup>
  import { computed } from 'vue'
  import { useI18n } from 'vue-i18n'

  const props = defineProps({
    file: Object,
    repoType: String,
    isCollection: Boolean,
    cardType: {
      type: String,
      default: 'index'
    }
  })

  const { t, locale } = useI18n()

  const detailLink = computed(() => {
    switch (props.repoType) {
      case 'model':
        return `/models/${props.file.path}`
      case 'dataset':
        return `/datasets/${props.file[`repo_namespace`]}/${props.file[`repo_name`]}/blob/${props.file.ref}/${props.file.path}`
      case 'space':
        return `/spaces/${props.file.path}`
      case 'code':
        return `/codes/${props.file.path}`
      default:
        return ''
    }
  })
</script>

<style scoped>
  .dataset-card:hover .dataset-path {
    color: var(--blue-blue-5001-f-75-cb, #1F75CB);
  }

  .model-card:hover .model-path {
    color: var(--theme-dark-red-t-red-500-ad-4-a-3-b, #AD4A3B);
  }

  .code-card:hover .code-path {
    color: var(--blue-blue-5001-f-75-cb, #1F75CB);
  }
</style>
