<template>
  <div class="fly-panel">
    <div class="fly-panel-title">Events</div>
    <div class="fly-panel-main">
      <a
        href="https://www.eurostep.com/announcement-export-control-complimentary-webinar/"
        target="_blank"
        class="fly-zanzhu">
        <div class="magazine-thumbnail">
          <img width="58" src="https://455767.smushcdn.com/1101216/wp-content/uploads/2020/06/plmroadmappdt2020fall-80x80.png?lossy=1&amp;strip=1&amp;webp=1">
        </div>
        <div class="av-magazine-content-wrap">
          <div class="date">25/06/2020</div>
          <div class="content">Sharing Export Control and ITAR information across an extended enterprise.</div>
        </div>
      </a>
      <a
        href="https://www.eurostep.com/shareaspace-forum-2020-thursday-june-4th-2020-200-pm-cest/"
        target="_blank"
        class="fly-zanzhu">
        <div class="magazine-thumbnail">
          <img width="58" src="https://455767.smushcdn.com/1101216/wp-content/uploads/2020/05/ShareAspace-Forum-2020-80x80.jpg?lossy=1&amp;strip=1&amp;webp=1">
        </div>
        <div class="av-magazine-content-wrap">
          <div class="date">31/08/2020</div>
          <div class="content">ShareAspace forum 2020 – Thursday, June 4th 2020 – 2:00 PM (CEST)</div>
        </div>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ads'
}
</script>

<style lang="scss" scoped>
.fly-zanzhu {
  display: flex;
  background-color: #ffffff;
  padding: 0;
  color: inherit;
}
.magazine-thumbnail {
  width: 58px;
  height: 58px;
  border: 1px solid #e5e5e5;
  margin-right: 15px;
}
.av-magazine-content-wrap {
  height: 58px;
  text-align: left;
  .date {
    height: 30px;
    line-height: 30px;
    font-size: 12px;
  }
  .content {
    max-width: 275px;
    height: 30px;
    line-height: 30px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
}
</style>
