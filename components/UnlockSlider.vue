<template>
  <GridLayout
    :background="containerBackgroundColor"
    width="100%"
    margin="0"
    padding="2"
    :borderRadius="radius"
    :height="buttonHeight"
  >
    <Label
      ref="infoText"
      width="100%"
      textAlignment="center"
      margin="0"
      padding="0"
      :color="infoTextColor"
      :fontSize="infoTextSize"
      :text="infoText"
      verticalAlignment="center"
      :opacity="1 - slidePercent * 1.5"
    />
    <Button
      ref="slideButton"
      class="slideButton"
      @pan="slide"
      z-index="0"
      :borderRadius="radius"
      :width="buttonHeight"
      :height="buttonHeight"
      :color="buttonTextColor"
      :fontWeight="buttonTextFontWeight"
      :backgroundColor="buttonBackgroundColor"
      margin="0"
      padding="0"
      :fontSize="buttonTextSize"
      :text="buttonText"
      horizontalAlignment="left"
    />
  </GridLayout>
</template>

<script>
export default {
  methods: {
    reset() {
      this.$refs.slideButton.nativeView
        .animate({
          translate: { x: 0, y: 0 },
          duration: 100,
        })
        .then(() => {
          this.slidePercent = 0;
        });
    },
    slide(args) {
      const buttonView = this.$refs.slideButton.nativeView;
      const gridViewWidth =
        buttonView.parent.getActualSize().width -
        buttonView.parent.paddingLeft -
        buttonView.parent.paddingRight;
      if (args.state === 1) {
        // Start paning
        this.prevDeltaX = 0;
        this.slidePercent = 0;
      } else if (args.state === 2) {
        // Paning
        buttonView.translateX += args.deltaX - this.prevDeltaX;
        this.prevDeltaX = args.deltaX;
        if (buttonView.translateX < 0) {
          buttonView.translateX = 0;
          this.prevDeltaX = 0;
        } else {
          const buttonPosition = this.buttonHeight + buttonView.translateX;
          if (buttonPosition > gridViewWidth) {
            buttonView.translateX = gridViewWidth - this.buttonHeight;
            this.prevDeltaX = 0;
          }
        }
        this.slidePercent =
          (this.buttonHeight + buttonView.translateX) / gridViewWidth;
      } else if (args.state === 3) {
        // Done paning
        if (gridViewWidth !== this.buttonHeight + buttonView.translateX) {
          this.reset();
        }
      }
    },
  },
  data() {
    return {
      prevDeltaX: 0,
      slidePercent: 0,
    };
  },
  watch: {
    slidePercent: function () {
      this.$emit("change", this.slidePercent);
    },
  },
  computed: {
    buttonHeight() {
      return this.height - 2;
    },
  },
  props: {
    height: {
      type: Number,
      default: 70,
    },
    radius: {
      type: Number,
      default: 70,
    },
    containerBackgroundColor: {
      type: String,
      default: "lightgray",
    },
    buttonText: {
      type: String,
      default: "→",
    },
    buttonTextSize: {
      type: Number,
      default: 20,
    },
    buttonTextColor: {
      type: String,
      default: "black",
    },
    buttonTextFontWeight: {
      type: String,
      default: "normal",
    },
    buttonBackgroundColor: {
      type: String,
      default: "white",
    },
    infoText: {
      type: String,
      default: "Slide to unlock",
    },
    infoTextSize: {
      type: Number,
      default: 16,
    },
    infoTextColor: {
      type: String,
      default: "black",
    },
  },
};
</script>