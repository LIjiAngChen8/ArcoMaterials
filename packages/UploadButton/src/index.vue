<template>
  <button
    class="upload-btn"
    :class="[
      size ? 'upload-btn-size-' + size : '',
      type ? 'upload-btn-shape-' + type : '',
      disabled ? 'upload-btn-' + warchDisabled : '',
      {
        uploading: upLoading || warchLoading || warchSuccess,
        done: upDone || warchSuccess,
      },
    ]"
    :disabled="disabled"
    @click="handleClick"
  >
    <a class="upload-icon" :class="[size ? 'upload-icon-' + size : '']">
      <svg>
        <use xlink:href="#circle"></use>
      </svg>
      <svg>
        <use xlink:href="#arrow"></use>
      </svg>
      <svg>
        <use xlink:href="#check"></use>
      </svg>
    </a>
    <svg xmlns="http://www.w3.org/2000/svg" style="display: none">
      <symbol
        id="circle"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 16 16"
      >
        <circle cx="8" cy="8" r="6"></circle>
      </symbol>
      <symbol id="arrow" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 12 12">
        <path
          d="M2.7008908,5.37931459 L2.7008908,5.37931459 C2.9224607,5.60207651 3.2826628,5.60304283 3.50542472,5.38147293 C3.52232305,5.36466502 3.53814843,5.34681177 3.55280728,5.32801875 L5.34805194,3.02646954 L5.34805194,10.3480519 C5.34805194,10.7081129 5.63993903,11 6,11 L6,11 C6.36006097,11 6.65194806,10.7081129 6.65194806,10.3480519 L6.65194806,3.02646954 L8.44719272,5.32801875 C8.6404327,5.57575732 8.99791646,5.61993715 9.24565503,5.42669716 C9.26444805,5.41203831 9.28230129,5.39621293 9.2991092,5.37931459 L9.2991092,5.37931459 C9.55605877,5.12098268 9.57132199,4.70855346 9.33416991,4.43193577 L6.75918715,1.42843795 C6.39972025,1.00915046 5.76841509,0.960656296 5.34912761,1.32012319 C5.31030645,1.35340566 5.27409532,1.38961679 5.24081285,1.42843795 L2.66583009,4.43193577 C2.42867801,4.70855346 2.44394123,5.12098268 2.7008908,5.37931459 Z"
        ></path>
      </symbol>
      <symbol id="check" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 12 12">
        <path
          id="test"
          d="M4.76499011,6.7673683 L8.2641848,3.26100386 C8.61147835,2.91299871 9.15190114,2.91299871 9.49919469,3.26100386 C9.51164115,3.27347582 9.52370806,3.28637357 9.53537662,3.29967699 C9.83511755,3.64141434 9.81891834,4.17816549 9.49919469,4.49854425 L5.18121271,8.82537365 C4.94885368,9.05820878 4.58112654,9.05820878 4.34876751,8.82537365 L2.50080531,6.97362503 C2.48835885,6.96115307 2.47629194,6.94825532 2.46462338,6.93495189 C2.16488245,6.59321455 2.18108166,6.0564634 2.50080531,5.73608464 C2.84809886,5.3880795 3.38852165,5.3880795 3.7358152,5.73608464 L4.76499011,6.7673683 Z"
        ></path>
      </symbol>
    </svg>
    <ul v-if="$slots.default">
      <li><slot></slot></li>
      <li>{{ loadText }}</li>
      <li>{{ successText }}</li>
      <span class="none"><slot></slot></span>
      <span class="none">{{ successText }}</span>
    </ul>
  </button>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'UploadButton',
  components: {},
  props: {
    /**
     * 按钮大小
     */
    size: {
      type: String,
      default: 'medium',
    },
    /**
     * 按钮形状
     */
    type: {
      type: String,
      default: 'round',
    },
    /**
     * 加载状态的内容
     */
    loadText: {
      type: String,
      default: '加载中',
    },
    /**
     * 成功状态的内容
     */
    successText: {
      type: String,
      default: '成功',
    },
    /**
     * 是否为加载状态
     */
    loading: {
      type: Boolean,
      default: false,
    },
    /**
     * 是否为成功状态
     */
    success: {
      type: Boolean,
      default: false,
    },
    /**
     * 按钮是否禁用
     */
    disabled: {
      type: Boolean,
      default: false,
    },
    /**
     * 加载状态持续时间（毫秒）
     */
    loadingDuration: {
      type: Number,
      default: 3000,
    },
    /**
     * 成功状态持续时间（毫秒）
     */
    successDuration: {
      type: Number,
      default: 1600,
    },
  },
  emits: ['click'],
  data() {
    return {
      upLoading: false, // 加载状态
      upDone: false, // 完成状态
    };
  },
  computed: {
    warchLoading() {
      return !!this.loading;
    },
    warchSuccess() {
      return !!this.success;
    },
    warchDisabled() {
      return this.disabled ? 'disabled' : '';
    },
  },
  methods: {
    handleClick(e: any) {
      const etime = this.successDuration;
      const ltime = this.loadingDuration;
      this.upLoading = true;
      setTimeout(() => {
        this.upDone = true;
        setTimeout(() => {
          this.upLoading = false;
          this.upDone = false;
        }, etime);
      }, ltime);
      this.$emit('click', e);
    },
  },
});
</script>
