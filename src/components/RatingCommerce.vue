<template>
    <div class="rating" :class="gender">
        <span v-for="(circle, index) in circles" :key="index" :class="{ filled: index < rating }"></span>
    </div>
</template>

<script>
    export default {
        name: 'RatingCommerce',
        props: {
            rating: {
                type: Number,
                required: true,
                validator: value => value >= 0 && value <= 5
            },
            gender: {
                type: String,
                required: true,
                validator: value => ['male', 'female'].includes(value)
            }
        },
        computed: {
            circles() {
                return Array.from({ length: 5 }, (_, i) => i < this.rating)
            }
        }
    }
</script>

<style scoped>
    .rating {
        display: flex;
        position: absolute;
        top: 32.8%;
        left: 85%;
    }

    .rating span {
        display: inline-block;
        width: 16px;
        height: 16px;
        border: 1px solid #ccc;
        border-radius: 50%;
        margin-right: 4px;
    }

    .rating span.filled {
        background-color: var(--rating-color, #ff9800);
        border-color: var(--rating-color, #ff9800);
    }

    .rating.male span.filled {
        --rating-color: #002772;
    }

    .rating.female span.filled {
        --rating-color: #720060;
    }
</style>