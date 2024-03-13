<template>
    <div class="main-parent">
        <img class="main-img" :src="getImageUrl()" :alt="metaProps.title || 'Enter valid image url'"/>

        <div class="title-div">
            <p class="page-title">{{ props.metaProps.title || 'Pratik Chakraborty | portfolio' }}</p>
            <p class="page-description">{{ props.metaProps.description || 'Hi, I am Pratik Chakraborty and you are currently viewing my portfolio website. Check now!' }}</p>
            <p class="url-title">{{ props.metaProps.url || 'pratikchakraborty.in' }}</p>
        </div>

    </div>
</template>

<script setup>
const props = defineProps(["metaProps"]);

// Provide default values if the original values are not available
const getImageUrl = () => {
    const defaultImageUrl = 'https://nuxt-portfolio.s3.ap-south-1.amazonaws.com/static-assets/Screenshot+2024-01-07+041541.png'; // Replace with your default image URL
    return props.metaProps.image || defaultImageUrl;
};
</script>

<style scoped>
.main-parent {
    height: 100%;
    /* width: 100%; */
    border-radius: 24px;
    display: flex;
    flex-direction: column;
    background-color: #ededeb;
}

.main-img {
    height: 72%;
    /* width: 100%; */
    border-radius: 24px 24px 0 0;
    object-fit: cover;
    min-height: 72%;
    cursor: pointer;
}

.title-div {
    height: 25%;
    width: 100%;
    border: 0px solid #d6d5d2;
    display: flex;
    flex-direction: column;
    gap: 5px;
    padding: 14px 0 14px 14px;

    .url-title {
        padding: 0;
        margin: 0;
        color: #9e9e9e;
        font-family: Inter;
        font-size: 16px;
        /* text-transform: uppercase; */
    }
    .page-title {
        padding: 0;
        margin: 0;
        font-weight: bold;
        font-size: 18px;
        font-family: Roboto;
    }

    .page-description {
    overflow: hidden;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    white-space: pre-line; /* Update this line */
    text-overflow: ellipsis;
    padding: 0;
    margin: 0;
    font-size: 14px;
    padding-right: 28px;
    color: #3d3d3d;
    font-family: Poppins;
  }
}
</style>