<template>
  <footer>
    <div class="container">
      <div class="contact">
        <div>
          <h2>聯絡方法</h2>
          <ul>
            <li>地址：</li>
            <li>聯絡電話：</li>
            <li>電子信箱：</li>
          </ul>
        </div>
        <div class="contact__media">
          <h3>原型實物</h3>
          <div>
            <a href="#">
              <i class="fab fa-facebook-square"></i>
            </a>
            <a href="#">
              <i class="fab fa-instagram"></i>
            </a>
            <a href="#">
              <i class="fab fa-line"></i>
            </a>
          </div>
        </div>
      </div>
    </div>
    <div class="copyright">&copy; 2020 原型實物</div>
  </footer>
</template>

<script>
export default {
  name: 'LayoutFooter',
  data () {
    return {
      cart: ''
    }
  }
}
</script>
