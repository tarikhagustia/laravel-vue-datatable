<template>
    <div :class="tableContainerClasses">
        <table :class="tableClasses">
            <thead :class="tableHeaderClasses">
                <tr :class="tableRowClasses">
                    <th
                        scope="col"
                        :key="column.name"
                        v-for="column in columns"
                        :class="headerClasses(column)"
                        :style="'width: ' + column.width + '%'"
                        @click="column.orderable  ? sort(column) : null">
                        <div class="inline-block" v-if="column.orderable">
                            <div
                                class="filter-asc"
                                :class="{'active-filter-asc': column.orderable && column.name == currentSort && dir == 'asc' }">
                            </div>
                            <div
                                class="filter-desc"
                                :class="{'active-filter-desc': column.orderable && column.name == currentSort && dir == 'desc' }">
                            </div>
                        </div>
                        {{ column.label }}
                    </th>
                </tr>
            </thead>
            <slot>
            </slot>
        </table>
    </div>
</template>

<style>

.filter-asc {
    width: 0;
    height: 0;
    border-left: 5px solid transparent;
    border-right: 5px solid transparent;
    border-bottom: 5px solid #ccc;
    margin-bottom: 1px;
}

.filter-desc {
    width: 0;
    height: 0;
    border-left: 5px solid transparent;
    border-right: 5px solid transparent;
    border-top: 5px solid #ccc;
    margin-top: 1px;
}

.active-filter-asc {
    border-bottom: 5px solid #a3a3a3;
}
.active-filter-desc {
    border-top: 5px solid #a3a3a3;
}

.inline-block {
    display: inline-block;
}

.table-header-sorting {
    cursor: pointer;
}

</style>

<script>
export default {
    data() {
        return {
            currentSort: '',
        };
    },
    props: {
        dir: {
            type: String,
            default: '',
        },  
        columns: {
            type: Array,
            default: () => ([]),
            required: true,
        },
        sortKey: {
            type: String,
            default: '',
        },
        sortOrders :{
            type: Object,
            default: () => ({}),
        },
        tableClasses: {
            type: Object,
            default: () => ({}),
        },
        tableHeaderClasses: {
            type: Object,
            default: () => ({
                'p-3': true,
                'text-left': true,
            }),
        },
        tableRowClasses: {
            type: Object,
            default: () => ({}),
        },
        tableContainerClasses: {
            type: Object,
            default: () => ({}),
        },
    },
    methods: {
        headerClasses(column) {
            let classes = this.tableHeaderClasses;
            classes['table-header-sorting'] = column.orderable;
            return classes;
        },
        sort(column) {
            this.currentSort = column.name;
            this.$emit('sort', column.name, column.columnName);
        },
    },
}
</script>
