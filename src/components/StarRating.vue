<template>
  <div
      ref="ratingRef"
      class="star-rating"
      :style="`--stars-between: ${starsBetween}px; --star-size: ${starSize}px;`"
      @mouseenter="enterRatingHandler"
      @mousemove="moveRatingHandler"
      @mouseleave="leaveRatingHandler"
      @click="setRatingHandler"
  >
    <div class="star-rating__items">
      <div
          v-for="index in starLimit"
          :key="index"
          ref="starsRef"
          class="star"
      >
        <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
          <g clip-path="url(#clip0_2697_67067)">
            <path
                d="M3.61163 15.443C3.22563 15.641 2.78763 15.294 2.86563 14.851L3.69563 10.121L0.17263 6.76501C-0.15637 6.45101 0.01463 5.87701 0.45563 5.81501L5.35363 5.11901L7.53763 0.792012C7.73463 0.402012 8.26763 0.402012 8.46463 0.792012L10.6486 5.11901L15.5466 5.81501C15.9876 5.87701 16.1586 6.45101 15.8286 6.76501L12.3066 10.121L13.1366 14.851C13.2146 15.294 12.7766 15.641 12.3906 15.443L7.99963 13.187L3.61063 15.443H3.61163Z"/>
          </g>
          <defs>
            <clipPath id="clip0_2697_67067">
              <rect width="16" height="16" fill="white"/>
            </clipPath>
          </defs>
        </svg>
      </div>
    </div>
    <div
        v-if="showCurrentValue"
        class="star-rating__value"
        :style="`width: ${valueRatingWidth}px`"
    >
      <div
          v-for="index in starLimit"
          :key="index"
          class="star filled"
      >
        <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
          <g clip-path="url(#clip0_2697_67067)">
            <path
                d="M3.61163 15.443C3.22563 15.641 2.78763 15.294 2.86563 14.851L3.69563 10.121L0.17263 6.76501C-0.15637 6.45101 0.01463 5.87701 0.45563 5.81501L5.35363 5.11901L7.53763 0.792012C7.73463 0.402012 8.26763 0.402012 8.46463 0.792012L10.6486 5.11901L15.5466 5.81501C15.9876 5.87701 16.1586 6.45101 15.8286 6.76501L12.3066 10.121L13.1366 14.851C13.2146 15.294 12.7766 15.641 12.3906 15.443L7.99963 13.187L3.61063 15.443H3.61163Z"/>
          </g>
          <defs>
            <clipPath id="clip0_2697_67067">
              <rect width="16" height="16" fill="white"/>
            </clipPath>
          </defs>
        </svg>
      </div>
    </div>
    <div
        v-if="!showCurrentValue"
        class="star-rating__plug"
        :style="`width: ${plugRatingWidth}px`"
    >
      <div
          v-for="index in starLimit"
          :key="index"
          class="star filled"
      >
        <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
          <g clip-path="url(#clip0_2697_67067)">
            <path
                d="M3.61163 15.443C3.22563 15.641 2.78763 15.294 2.86563 14.851L3.69563 10.121L0.17263 6.76501C-0.15637 6.45101 0.01463 5.87701 0.45563 5.81501L5.35363 5.11901L7.53763 0.792012C7.73463 0.402012 8.26763 0.402012 8.46463 0.792012L10.6486 5.11901L15.5466 5.81501C15.9876 5.87701 16.1586 6.45101 15.8286 6.76501L12.3066 10.121L13.1366 14.851C13.2146 15.294 12.7766 15.641 12.3906 15.443L7.99963 13.187L3.61063 15.443H3.61163Z"/>
          </g>
          <defs>
            <clipPath id="clip0_2697_67067">
              <rect width="16" height="16" fill="white"/>
            </clipPath>
          </defs>
        </svg>
      </div>
    </div>
  </div>
</template>

<script setup>
import {computed, onMounted, ref} from "vue";

const props = defineProps({
  rating: {
    type: [Number, String],
    default: 0
  },
  starLimit: {
    type: Number,
    default: 5
  },
  readOnly: {
    type: Boolean,
    default: false
  },
  step: {
    type: Number,
    default: 1
  },
  starSize: {
    type: Number,
    default: 48
  },
  starsBetween: {
    type: Number,
    default: 8
  }
});

const emit = defineEmits(['move-rating', 'update-rating'])

const currentPos = ref(0);
const ratingRef = ref(null);
const ratingWidth = ref(0);
const ratingLeft = ref(0);
const starsRef = ref([]);
const currentRating = ref(props.rating ?? 0);
const plugRatingWidth = ref(0);
const valueRatingWidth = ref(0);
const showCurrentValue = ref(false);

const maxSteps = computed(() => {
  return props.starLimit / props.step;
});

const stepSize = computed(() => {
  return ratingWidth.value / maxSteps.value;
});

const ratingDecimalLength = computed(() => {
  return props.step?.toString()?.split('.')?.[1]?.length ?? 0;
});

const lastRatingValue = computed(() => {
  return Math.ceil(currentPos.value / stepSize.value) * props.step;
});

const preparedRatingValue = computed(() => {
  return +lastRatingValue.value?.toFixed(ratingDecimalLength.value) ?? 0;
});

const enterRatingHandler = () => {
  if (!props.readOnly) {
    showCurrentValue.value = false;
  }
}

const leaveRatingHandler = () => {
  if (!props.readOnly) {
    showCurrentValue.value = true;
  }
}

const moveRatingHandler = (e) => {
  if (!props.readOnly) {
    currentPos.value = e.pageX - ratingLeft.value;
    setPlugRatingWidth();

    emit('move-rating', preparedRatingValue.value);
  }
}

const setRatingHandler = () => {
  if (!props.readOnly) {
    currentRating.value = preparedRatingValue.value;

    setValueRatingWidth();
    emit('update-rating', currentRating.value);
  }
}

const setPlugRatingWidth = () => {
  plugRatingWidth.value = calculateValueRatingWidth(lastRatingValue.value);
}

const setValueRatingWidth = () => {
  valueRatingWidth.value = calculateValueRatingWidth(currentRating.value);
}

const calculateValueRatingWidth = (value) => {
  const decimal = value % 1;
  const currentStarIndex = Math.ceil(value - 1);
  const currentStarElement = starsRef.value?.[currentStarIndex];

  if (currentStarElement) {
    const currentStarPosition = currentStarElement?.getBoundingClientRect();
    const starFillWidth = decimal ? (props.starSize * decimal) : props.starSize;

    return ((currentStarPosition?.left - ratingLeft.value) + starFillWidth) ?? 0;
  }

  return 0;
}

onMounted(() => {
  const ratingClientRect = ratingRef.value?.getBoundingClientRect();

  if (ratingClientRect) {
    ratingWidth.value = ratingClientRect?.width ?? 0;
    ratingLeft.value = ratingClientRect?.left ?? 0;

    if (currentRating.value) {
      setValueRatingWidth();
    }
  }
});
</script>

<style scoped lang="scss">
.star {
  width: var(--star-size);
  height: var(--star-size);
  flex-shrink: 0;

  svg {
    width: 100%;
    height: 100%;
    fill: #F3BB93;
  }

  &.filled {
    svg {
      fill: #ff6600;
    }
  }

  &-rating {
    position: relative;

    &__items {
      display: flex;
      gap: var(--stars-between);
    }

    &__plug, &__value {
      position: absolute;
      display: flex;
      gap: var(--stars-between);
      top: 0;
      left: 0;
      width: 0;
      overflow: hidden;
    }
  }
}
</style>
