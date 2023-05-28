<template>
<!--  <div>-->
<!--    <svg-->
<!--        :width="size"-->
<!--        :height="size"-->
<!--        fill="none"-->
<!--        :viewBox="viewBox"-->
<!--        xmlns="http://www.w3.org/2000/svg"-->
<!--    >-->
<!--      <path-->
<!--          v-for="source in pathSourceOld"-->
<!--          :key="source"-->
<!--          :d="source"-->
<!--          fill="currentColor"-->
<!--      />-->
<!--    </svg>-->
<!--    {{getIconName}}-->
<!--  </div>-->

  <img class="icon" :src="pathSource" />
</template>
<script>
export default {
  props: {
    icon: {
      type: String,
      required: true,
    },
    icons: {
      type: Object,
      required: true,
    },
    size: {
      type: [String, Number],
      default: '20',
    },
    type: {
      type: String,
      default: 'outline',
    },
    viewBox: {
      type: String,
      default: '0 0 24 24',
    },
  },
  data() {
    return {
      image: '',
      iconName: '',
    };
  },
  computed: {
    getIconName(){
      return this.iconName;
    },
      pathSourceOld(){
          // To support icons with multiple paths
        this.iconName = this.icon;
          const path = this.icons[`${this.icon}-${this.type}`];
          if (path.constructor === Array) {
            return path;
          }
          return [path];
      },
    pathSource() {
      // To support icons with multiple paths
      // const path = this.icons[`${this.icon}-${this.type}`];
      // if (path.constructor === Array) {
      //   return path;
      // }
      // return [path];
      switch (this.icon) {
        case 'chat':
          return require('../../assets/images/chat_icon.svg');
        case 'wifi-off':
          return require('../../assets/images/chat_icon.svg');
        case 'dismiss-circle':
          return require('../../assets/images/chat_icon.svg');
        case 'book-contacts':
          return require('../../assets/images/contacts_icon.svg');
        case 'arrow-trending-lines':
          return require('../../assets/images/reports_icon.svg');
        case 'megaphone':
          return require('../../assets/images/campaigns_icon.svg');
        case 'library':
          return require('../../assets/images/chat_icon.svg');
        case 'settings':
          return require('../../assets/images/settings_icon.svg');
        case 'alert':
          return require('../../assets/images/notification_icon.svg');
        case 'mention':
          return require('../../assets/images/chat_icon.svg');
        case 'mail-unread':
          return require('../../assets/images/chat_icon.svg');
        case 'add':
          return require('../../assets/images/chat_icon.svg');
        case 'list':
          return require('../../assets/images/chat_icon.svg');
        case 'search':
          return require('../../assets/images/chat_icon.svg');
        case 'arrow-right-import':
          return require('../../assets/images/chat_icon.svg');
        case 'filter':
          return require('../../assets/images/chat_icon.svg');
        case 'sort-icon':
          return require('../../assets/images/chat_icon.svg');
        case 'mail':
          return require('../../assets/images/chat_icon.svg');
        case 'arrow-reply':
          return require('../../assets/images/chat_icon.svg');
        case 'services':
          return require('../../assets/images/services_icon.svg');
        case 'schedule':
          return require('../../assets/images/schedule_icon.svg');
        case 'dashboard':
          return require('../../assets/images/dashboard_icon.svg');
        default:
          return require('../../assets/images/chat_icon.svg');
      }
    },
  },
};
</script>
