<script>
  // https://dev.to/bmw2621/build-an-image-carousel-with-svelte-4kf8
  import { onDestroy } from "svelte";
  import { flip } from "svelte/animate";
  import { fade } from "svelte/transition";
  import img1 from "/gallery1/IMG_7790.jpg";
  import img2 from "/gallery1/IMG_7791.jpg";

  export let imagePadding = 3;
  export let imageWidth = 75;
  export let transitionSpeed = 1000;
  export let autoplay = true;
  export let autoplaySpeed = 1000;

  let imagePaddingString = `padding: ${imagePadding}px;`;
  let imageWidthString = `width: ${imageWidth}%`;
  let imageClass = `${imageWidthString} ${imagePaddingString}`;
  let interval;

  let images = [
    {
      path: img1,
      id: "image1",
    },
    {
      path: img2,
      id: "image2",
    },
    {
      path: img2,
      id: "image3",
    },
    {
      path: img2,
      id: "image4",
    },
    {
      path: img2,
      id: "image5",
    },
  ];

  const rotateLeft = (e) => {
    const transitioningImage = images[images.length - 1];
    document.getElementById(transitioningImage.id).style.opacity = 0;
    images = [images[images.length - 1], ...images.slice(0, images.length - 1)];
    setTimeout(() => {
      document.getElementById(transitioningImage.id).style.opacity = 1;
    }, transitionSpeed);
  };

  const rotateRight = (e) => {
    const transitioningImage = images[0];
    document.getElementById(transitioningImage.id).style.opacity = 0;
    images = [...images.slice(1, images.length), images[0]];
    setTimeout(() => {
      document.getElementById(transitioningImage.id).style.opacity = 1;
    }, transitionSpeed);
  };

  const startAutoPlay = () => {
    if (autoplay) {
      interval = setInterval(rotateRight, autoplaySpeed);
    }
  };

  const stopAutoplay = () => {
    clearInterval(interval);
  };

  if (autoplay) {
    startAutoPlay();
  }

  onDestroy(() => {
    stopAutoplay;
  });
</script>

<div class="flex w-full relative flex-col overflow-hidden">
  <div class="flex justify-center">
    {#each images as image (image.id)}
      <img
        src={image.path}
        alt={image.id}
        id={image.id}
        style={`${imagePaddingString} ${imageWidthString}`}
        transition:fade
        animate:flip={{ duration: transitionSpeed }}
      />
    {/each}
  </div>
  <button
    class="absolute left-2 top-1/2 text-5xl text-gray-100 opacity-70"
    on:click={rotateLeft}>&#x2770;</button
  >
  <button
    class="absolute right-2 top-1/2 text-5xl text-gray-100 opacity-70"
    on:click={rotateRight}>&#x2771;</button
  >
</div>
