<script>
  import {
    isOzaria,
  } from 'core/utils'
  /**
   * Renderless component that manages default head tag configuration for vue-meta.
   */
  export default {
    props: {
      currentQueryParams: {
        type: Object,
        default: () => ({})
      }
    },

    metaInfo: function () {
      const links = []
      const utmKeys = Object
        .keys(this.currentQueryParams || {})
        .filter(k => k.startsWith('utm_'))

      if (utmKeys.length > 0) {
        const urlWithoutUtm = new URL(window.location.href)

        const urlSearchParams = new URLSearchParams(urlWithoutUtm.search)
        utmKeys.forEach(k => urlSearchParams.delete(k))

        urlWithoutUtm.search = urlSearchParams.toString()
        links.push({ vmid: 'rel-canonical', rel: 'canonical', href: urlWithoutUtm.toString() })
      }

      return {
        title: this.$t('common.default_title'),
        ...(isOzaria ? {} : { titleTemplate: '%s | CodeCombat' }),
        meta: [
          { vmid: 'meta-description', name: 'description', content: this.$t('common.default_meta_description') },
          { vmid: 'viewport', name: 'viewport', content: 'width=device-width,initial-scale=1.0' }
        ],

        link: links
      }
    },

    render () {
      return this.$slots.default
    }
  }
</script>
