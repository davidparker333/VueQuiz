<template>
    <div class="row mb-5">
        <div class="col-12">
            <b-nav tabs align="center">
                <b-nav-item disabled>Fancy Vue App</b-nav-item>
                <b-nav-item disabled>Counter : {{ numCorrect }} correct / {{ numTotal }} total</b-nav-item>
            </b-nav>
        </div>
    </div>
</template>

<script>
export default {
    props: ['numCorrect', 'numTotal']
}
</script>
