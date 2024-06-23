<template>
  <footer id="footer" role="contentinfo">
    <div class="footer__inner container">
      <!-- Copyright Notice -->
      <div class="footer__copyright">
        <a href="mailto:rhthrck12@gmail.com">mail : rhthrck12@gmail.com</a>
        <p>
          본 웹사이트는 포트폴리오 용도로 제작되었으며, 사용된 모든 콘텐츠의 저작권은 TMDB에 있으며,
          이에 대한 모든 책임은 TMDB에 있습니다.
        </p>
      </div>
    </div>
  </footer>
</template>
<style lang="scss">
#footer {
  background-color: #16182f;
  color: #929292;
  padding: 20px 0;
  text-align: center;

  .footer__copyright {
    flex-grow: 1;
    text-align: center;
    padding-right: 20px; /* Adjust spacing */
  }
}
</style>
