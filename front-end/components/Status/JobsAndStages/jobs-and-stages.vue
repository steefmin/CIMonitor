<template>
    <div class="status__detail-jobs-and-stages">
        <div class="status__detail-stages" v-if="stages.length > 0">
            <div class="status__detail-stage" :class="getStageStatus(stage)" v-for="stage in stages" :key="stage">
                {{ stage }}
            </div>
        </div>
        <div class="status__detail-jobs" v-if="interestingJobs.length > 0">
            <div class="status__detail-job" v-for="job in interestingJobs" :class="job.state" :key="job.name">
                {{ job.name }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        jobs: {
            type: Array,
            default: () => [],
        },
        stages: {
            type: Array,
            default: () => [],
        },
    },
    methods: {
        getStageStatus(stage) {
            const jobsForStage = this.jobs.filter(job => job.stage && job.stage === stage);

            if (jobsForStage.find(job => job.state === 'running')) {
                return 'running';
            }

            if (jobsForStage.find(job => job.state === 'error')) {
                return 'error';
            }

            if (jobsForStage.find(job => job.state === 'warning')) {
                return 'warning';
            }

            if (jobsForStage.find(job => job.state === 'success')) {
                return 'success';
            }

            return 'pending';
        },
    },
    computed: {
        interestingJobs() {
            if (!this.jobs) {
                return [];
            }

            return this.jobs.filter(job => ['running', 'error', 'warning'].indexOf(job.state) !== -1);
        },
    },
};
</script>

<style lang="sass" rel="stylesheet/sass" scoped>
.status__detail-jobs,
.status__detail-stages
    font-size: 26px
</style>
