<template>
    <div class="day-cell"
         :class="{'today' : day.isToday, 'current-month' : day.isCurrentMonth, 'weekend': day.isWeekEnd, 'selected-day':showDayOptionsFlag}"
    @click="showDayOptions">
        <div class="row">
            <div class="col-sm-6">
                <div v-show="showDayOptionsFlag">
                    <span class="label label-success" @click="showAddEventForm"> {{ $t('generic.add_event') }}</span>
                </div>
            </div>
            <div class="col-sm-6">
                <p class="day-number">{{ day.date.format('D') }}</p>
            </div>
        </div>
        <div class="event-box">
            <EventCard
                    :event="event"
                    :key="event.id"
                    :date="day.date"
                    :first-day="firstDay"
                    v-for="event in day.events">
            </EventCard>
        </div>
    </div>
</template>
<script>
    import moment from 'moment';
    import {DAY_SELECTED, CHANGE_MONTH} from '../actions';
    export default {
        data () {
            return {
                showDayOptionsFlag: false
            }
        },
        components: {
            'EventCard' : require('./EventCard.vue'),
        },
        props:{
            day: {
                type: Object
            },
            event: {
                type: Object
            },
            firstDay: {
                type: String
            },
        },
        created(){
            let me = this;
            EventsBus.$on(DAY_SELECTED, function (payload) {
                if(payload.dayDate != me.day.date) {
                    me.showDayOptionsFlag = false;
                }
            });
            EventsBus.$on(CHANGE_MONTH, function () {
                me.showDayOptionsFlag = false;
            });
        },
        computed: {
        },
        methods : {
            showDayOptions(){
                let startOfToday = moment().startOf('day');
                if(this.day.date.isAfter(startOfToday) || this.day.date.isSame(startOfToday)) {
                    let me = this;
                    me.showDayOptionsFlag = true;
                    EventsBus.$emit(DAY_SELECTED, {dayDate:me.day.date});
                }
            },
            showAddEventForm(){
                alert('I will leave this to you to implement :)');
            }
        }
    }
</script>
<style>
    .day-cell {
        flex: 1;
        min-height: 112px;
        padding: 4px;
        border-right: 1px solid #e0e0e0;
        border-bottom: 1px solid #e0e0e0;
        background: rgba(147, 147, 147, 0.1);
    }

    .day-number {
        text-align: right;
        color: rgba(0, 0, 0, .25);
        font-size: 1em;
        padding: 5px;
    }

    .current-month {
        background: #fff;
    }

    .current-month p {
        color: rgba(0, 0, 0, .5);
        font-size: 1.5em;
    }

    .selected-day p {
        font-size: 2.4em;
        font-weight: bolder;
    }

    .weekend p {
        color: rgba(210, 2, 2, 0.6);
    }

    .today {
        background-color: #e8fde7;
    }

    .today p {
        font-size: 2em;
        font-weight: bolder;
        color: #367016;
    }
</style>