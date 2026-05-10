<template>
  <div class="links-card">
    <div class="header">
      <Icon icon="ri:compass-3-line" width="20" height="20" />
      <span class="title">项目导航</span>
    </div>

    <swiper
      :modules="[Pagination, Mousewheel]"
      :slides-per-view="1"
      :space-between="20"
      :pagination="siteLinksList.length > 1 ? { clickable: true } : false"
      :mousewheel="true"
      class="link-swiper"
    >
      <swiper-slide v-for="(page, pIndex) in siteLinksList" :key="pIndex">
        <div class="link-grid">
          <a
            v-for="(item, index) in page"
            :key="index"
            :href="getPrimaryLink(item)"
            target="_blank"
            rel="noopener noreferrer"
            class="link-item glass-card"
          >
            <div class="card-top">
              <div class="icon-box">
                <Icon :icon="item.icon || 'ri:link'" width="28" height="28" />
              </div>
              <div class="badge-row">
                <span v-if="item.status" class="status-badge">{{ item.status }}</span>
                <span v-if="item.tag" class="tag-badge">{{ item.tag }}</span>
                <Icon icon="ri:arrow-right-up-line" width="18" height="18" class="open-icon" />
              </div>
            </div>

            <div class="content-box">
              <span class="name">{{ item.name }}</span>
              <span class="desc">{{ item.desc || '点击访问项目主页' }}</span>
            </div>

            <div class="action-row">
              <span v-if="item.link" class="action-btn primary">
                <Icon icon="ri:external-link-line" width="14" height="14" />
                访问主页
              </span>
              <a
                v-if="item.github"
                :href="item.github"
                target="_blank"
                rel="noopener noreferrer"
                class="action-btn secondary"
                @click.stop
              >
                <Icon icon="ri:github-line" width="14" height="14" />
                GitHub
              </a>
            </div>
          </a>
        </div>
      </swiper-slide>
    </swiper>
  </div>
</template>

<script setup>
import { computed } from 'vue';
import { Swiper, SwiperSlide } from 'swiper/vue';
import { Pagination, Mousewheel } from 'swiper/modules';
import 'swiper/css';
import 'swiper/css/pagination';
import { Icon } from '@iconify/vue';
import { siteLinks } from '@/config';

const pageSize = 4;

const siteLinksList = computed(() => {
  const result = [];
  for (let i = 0; i < siteLinks.length; i += pageSize) {
    result.push(siteLinks.slice(i, i + pageSize));
  }
  return result;
});

const getPrimaryLink = (item) => item.link || item.github || '#';
</script>

<style scoped lang="scss">
.links-card {
  width: 100%;
  margin-top: 0;

  .header {
    display: flex;
    align-items: center;
    margin-bottom: 15px;
    color: white;
    .title { margin-left: 8px; font-weight: bold; font-size: 1.1rem; }
  }

  .link-swiper {
    width: 100%;
    padding-bottom: 30px;

    :deep(.swiper-pagination-bullet) {
      background: #fff;
      opacity: 0.4;
      &.swiper-pagination-bullet-active { opacity: 1; }
    }
  }

  .link-grid {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 18px;

    @media (max-width: 720px) {
      grid-template-columns: 1fr;
    }

    .link-item {
      min-height: 176px;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      color: white;
      text-decoration: none;
      transition: transform 0.28s ease, background 0.28s ease, border-color 0.28s ease, box-shadow 0.28s ease;
      padding: 18px;
      overflow: hidden;

      &:hover {
        background: rgba(0, 0, 0, 0.38);
        border-color: rgba(255, 255, 255, 0.22);
        box-shadow: 0 12px 30px rgba(0, 0, 0, 0.22);
        transform: translateY(-4px);

        .icon-box {
          background: rgba(125, 211, 252, 0.18);
          color: #7dd3fc;
          transform: scale(1.06);
        }

        .open-icon {
          opacity: 1;
          transform: translate(2px, -2px);
        }
      }

      &:active {
        transform: scale(0.98);
      }

      .card-top {
        display: flex;
        align-items: flex-start;
        justify-content: space-between;
        margin-bottom: 14px;
      }

      .icon-box {
        width: 46px;
        height: 46px;
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: 14px;
        background: rgba(255, 255, 255, 0.12);
        color: rgba(255, 255, 255, 0.94);
        transition: 0.28s ease;
      }

      .open-icon {
        opacity: 0.55;
        transition: 0.28s ease;
      }

      .badge-row {
        display: flex;
        align-items: center;
        justify-content: flex-end;
        gap: 6px;
        min-width: 0;
      }

      .tag-badge,
      .status-badge {
        height: 24px;
        display: inline-flex;
        align-items: center;
        border-radius: 999px;
        padding: 0 8px;
        font-size: 0.72rem;
        line-height: 1;
        white-space: nowrap;
      }

      .tag-badge {
        color: rgba(255, 255, 255, 0.76);
        background: rgba(255, 255, 255, 0.08);
        border: 1px solid rgba(255, 255, 255, 0.1);
      }

      .status-badge {
        color: #fde68a;
        background: rgba(251, 191, 36, 0.14);
        border: 1px solid rgba(251, 191, 36, 0.22);
      }

      .content-box {
        display: flex;
        flex-direction: column;
        gap: 7px;
        min-width: 0;
      }

      .name {
        font-size: 1.08rem;
        font-weight: 700;
        line-height: 1.2;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
      }

      .desc {
        min-height: 54px;
        color: rgba(255, 255, 255, 0.66);
        font-size: 0.86rem;
        line-height: 1.45;
        display: -webkit-box;
        -webkit-line-clamp: 3;
        -webkit-box-orient: vertical;
        overflow: hidden;
      }

      .action-row {
        display: flex;
        gap: 10px;
        margin-top: 16px;
      }

      .action-btn {
        min-width: 0;
        height: 30px;
        display: inline-flex;
        align-items: center;
        justify-content: center;
        gap: 5px;
        border-radius: 9px;
        padding: 0 10px;
        color: white;
        font-size: 0.78rem;
        line-height: 1;
        text-decoration: none;
        white-space: nowrap;
        transition: 0.25s ease;
      }

      .primary {
        flex: 1;
        background: rgba(255, 255, 255, 0.16);
      }

      .secondary {
        border: 1px solid rgba(255, 255, 255, 0.16);
        background: rgba(255, 255, 255, 0.06);

        &:hover {
          background: rgba(255, 255, 255, 0.16);
          border-color: rgba(255, 255, 255, 0.3);
        }
      }
    }
  }
}

.glass-card {
  background: rgba(0, 0, 0, 0.25);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  border: 1px solid rgba(255, 255, 255, 0.1);
}
</style>
