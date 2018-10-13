<template>
  <div role='group' :class='groupClasses'>
    <button
      v-for='(option, index) in options'
      :key='index'
      type='button'
      :class='buttonClasses(option)'
      @click='select(option)'
      >
      {{ option[label] }}
    </button>
  </div>
</template>

<script>
import { eq } from "lodash";

const sizeClasses = {
  small: "btn-group-sm",
  medium: "",
  large: "btn-group-lg"
};

const typeClasses = {
  primary: "-primary",
  secondary: "-secondary",
  success: "-success",
  danger: "-danger",
  warning: "-warning",
  info: "-info",
  light: "-light",
  dark: "-dark",
  link: "-link"
};

const noDefaultValue = Symbol("ButtonRadioGroup.noDefaultValue");

export default {
  props: {
    value: {
      type: null,
      default: null
    },
    options: {
      type: Array,
      default: () => []
    },
    defaultValue: {
      type: null,
      default: noDefaultValue
    },
    layout: {
      type: String,
      default: "horizontal",
      validator(value) {
        return value === "horizontal" || value === "vertical";
      }
    },
    size: {
      type: String,
      default: "medium",
      validator(value) {
        return sizeClasses.hasOwnProperty(value);
      }
    },
    type: {
      type: String,
      default: "primary",
      validator(value) {
        return typeClasses.hasOwnProperty(value);
      }
    },
    outline: {
      type: Boolean,
      default: false
    },
    label: {
      type: String,
      default: "label"
    },
    trackBy: {
      type: String,
      default: "value"
    }
  },
  computed: {
    groupClasses() {
      return {
        "btn-group": this.layout === "horizontal",
        "btn-group-vertical": this.layout === "vertical",
        [sizeClasses[this.size]]: true
      };
    }
  },
  methods: {
    select(option) {
      if (
        this.defaultValue !== noDefaultValue &&
        eq(this.value, option[this.trackBy])
      ) {
        this.$emit("input", this.defaultValue);
      } else {
        this.$emit("input", option[this.trackBy]);
      }
    },
    isSelected(option) {
      return eq(this.value, option[this.trackBy]);
    },
    buttonClasses(option) {
      const outline = this.outline ? "-outline" : "";
      return {
        btn: true,
        active: this.isSelected(option),
        ["btn" + outline + typeClasses[this.type]]: true
      };
    }
  }
};
</script>