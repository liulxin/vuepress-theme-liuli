<template>
  <section id="header-wrapper">
    <header id="header" :class="scrollTop > 10 ? 'headshadow' : ''">
      <div class="header-wrapper">
        <div class="title">
          <NavLink link="/" class="home-link">{{ $site.title }} </NavLink>
        </div>
        <div class="header-right-wrap">
          <ul v-if="$themeConfig.nav" class="nav">
            <li
              v-for="item in $themeConfig.nav"
              :key="item.text"
              class="nav-item"
            >
              <NavLink :link="item.link">{{ item.text }}</NavLink>
            </li>
          </ul>
          <SearchBox />
        </div>
      </div>
    </header>
  </section>
</template>

<script>
import SearchBox from '@SearchBox'
import debounce from 'lodash.debounce'

export default {
  components: { SearchBox },
  data() {
    return {
      scrollTop: null,
    }
  },
  mounted() {
    this.scrollTop = this.getScrollTop()
    window.addEventListener(
      'scroll',
      debounce(() => {
        this.scrollTop = this.getScrollTop()
      }, 100)
    )
  },
  methods: {
    getScrollTop() {
      return (
        window.pageYOffset ||
        document.documentElement.scrollTop ||
        document.body.scrollTop ||
        0
      )
    },
  },
}
</script>

<style lang="stylus">
@import '~@app/style/config'

.headshadow
  box-shadow 0 5px 20px rgba(0, 0, 0, 0.03), 0 6px 6px rgba(0, 0, 0, 0.05)

#header
  z-index 12
  position fixed
  top 0
  width 100vw
  box-sizing border-box
  background-color #FFF
  padding 20px 32px 20px
  margin auto
  border-bottom 1px solid  rgba(0, 0, 0, 0.08)
  transition all 1s cubic-bezier(0.25, 0.8, 0.25, 1)

  ol, ul
    list-style none
    margin 0
    padding 0

  &:hover
    box-shadow 0 5px 20px rgba(0, 0, 0, 0.08), 0 6px 6px rgba(0, 0, 0, 0.1)

// border-bottom 5px solid lighten(#3eaf7c, 50%)
.header-wrapper
  display flex
  line-height 40px
  height 40px

  .title
    /* flex 0 0 200px */
    color #000
    font-size 30px
    margin 0
    letter-spacing 2px
    display block
    text-transform uppercase

    a
      color #000
      font-weight bold
      font-family PT Serif, Serif
      text-decoration none

  .header-right-wrap
    flex 1
    display flex
    justify-content flex-end

    .nav
      flex 0 0 auto
      display flex
      margin 0
      align-items center

      .nav-item
        margin-left 20px

        a
          font-family PT Serif, Serif
          font-size 20px
          // color lighten(#3eaf7c, 30%)
          text-decoration none
          transition color 0.3s

    .search-box
      font-family PT Serif, Serif
      margin-left 20px

      input
        border-radius 5px
        transition all 0.5s
        border 1px solid #cecece

        &:hover
          border 1px solid $accentColor
          box-shadow 0 0 5px $accentColor

      .suggestions
        border 1px solid #000
        top 40px
        right 0

        a
          color #000
          text-decoration none

          &.focused
            color $accentColor

@media (max-width: $MQMobile)
  #header
    display none

  .header-wrapper
    flex-direction column

    .header-right-wrap
      display none
</style>
