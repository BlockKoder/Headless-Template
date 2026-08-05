<template>
    <Hero
        :title="$t('index.hero.title')"
        :subtitle="$t('index.hero.subtitle')"
        image="/sky.png"
        class="mb-8"
    />

    <div class="container">
        <div
            v-if="categories && categories.length > 3"
            class="row justify-center"
        >
            <div
                class="col-auto"
                v-for="category in categories"
                :key="category.id"
            >
                <NuxtLink
                    class="category-link"
                    tag="a"
                    :to="{
                        hash: `#${titleCase(category.name)}`,
                    }"
                >
                    <CategoryCard
                        :image="category.packages[0].image"
                        :category="category"
                    />
                </NuxtLink>
            </div>
        </div>

        <div class="d-flex my-5 w-100"></div>

        <div
            v-for="category in categories"
            :key="category.id"
            :id="titleCase(category.name)"
            class="category"
        >
            <HeaderCard>
                <NuxtImg
                    :src="category.packages[0].image"
                    :alt="category.name"
                    width="60px"
                    class="mr-3"
                />

                <h3>{{ category.name }}</h3>
            </HeaderCard>

            <div class="row">
                <div
                    class="col-12 col-sm-6 col-md-4 col-lg-3"
                    v-for="pkg in category.packages"
                    :key="pkg.id"
                >
                    <PackageCard
                        :pkg="pkg"
                        :row="category.packages.length === 1"
                        hide-options
                    />
                </div>
            </div>
        </div>
    </div>

    <!-- Holds the login modal -->
    <NuxtPage />
</template>

<script lang="ts" setup>
const { t } = useI18n();

useSeoMeta({
    ogTitle: t("index.hero.title"),
    description: t("index.hero.subtitle"),
    ogDescription: t("index.hero.subtitle"),
    ogImage: "/Eleria.png",
    twitterCard: "summary_large_image",
});

const categoryStore = useCategoryStore();
const { data: categories } = await useAsyncData("categories", () => {
    return categoryStore.fetchCategories();
});
</script>

<style scoped lang="scss">
.category {
    margin-bottom: 56px;

    :deep(.header-card) {
        margin-bottom: 18px;
        padding: 14px 18px;
        border: 1px solid rgba(143, 162, 194, 0.2);
        box-shadow: 0 14px 26px rgba(0, 0, 0, 0.16);

        h3 {
            letter-spacing: -0.01em;
            margin: 0;
        }
    }

    :deep(.package-card) {
        animation: card-reveal 500ms ease both;
    }

    :deep(.col-12:nth-child(2) .package-card) {
        animation-delay: 60ms;
    }

    :deep(.col-12:nth-child(3) .package-card) {
        animation-delay: 120ms;
    }

    :deep(.col-12:nth-child(4) .package-card) {
        animation-delay: 180ms;
    }

    &-link {
        text-decoration: none;
    }
}

@keyframes card-reveal {
    from {
        opacity: 0;
        transform: translateY(12px);
    }

    to {
        opacity: 1;
        transform: translateY(0);
    }
}
</style>
