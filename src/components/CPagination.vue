<template>
    <ul class="pagination center">
        <li class="pagination-item">
            <button v-html="preIcon" type="button" class="center" @click="onClickPreviousPage" :disabled="isInFirstPage"
                :style="[isInFirstPage ? disbledStyle : '', buttonStyle]">
            </button>
        </li>
        <li v-for="page in pages" :key="page" class="pagination-item">
            <button type="button" class="center" @click="onClickPage(page.name)" :disabled="page.isDisabled"
                :style="[buttonStyle,isPageActive(page.name) ? activeStyle : '']"> {{ page.name }} </button>
        </li>
        <li class="pagination-item">
            <button v-html="nextIcon" type="button" class="center" @click="onClickNextPage" :disabled="isInLastPage"
                :style="[isInLastPage ? disbledStyle : '', buttonStyle]">
            </button>
        </li>
    </ul>
</template>
  
<script>
export default {
    name: "CPagination",
    props: {
        maxVisibleButtons: {
            type: Number,
            required: false,
            default: 3
        },
        totalPages: {
            type: Number,
            required: true
        },
        perPage: {
            type: Number,
            required: true
        },
        currentPage: {
            type: Number,
            required: true
        },
        activeColor: {
            type: String,
            default: "#1dbf73"
        },
        activeBg: {
            type: String,
            default: '#FFFFFF'
        },
        disableBg: {
            type: String,
            default: "#6c757d"
        },
        height: {
            type: String,
            default: '32px'
        },
        width: {
            type: String,
            default: "32px"
        },
        fontSize: {
            type: String,
            default: "14px"
        },
        color: {
            type: String,
            default: "#000000"
        },
        preIcon: {
            type: String,
            default: '<img class=" " src="https://cdn.tradeinsur.com/assets/icon/grey-4-left.svg" alt="">'

        },
        nextIcon: {
            type: String,
            default: '<img class="left-arrow" src="https://cdn.tradeinsur.com/assets/icon/grey-4-left.svg" alt="">'

        }
    },
    data() {
        return {
            activeStyle: {
                border: `1px solid ${this.activeColor}`,
                color: this.activeColor,
                backgroundColor: this.activeBg
            },
            disbledStyle: {
                backgroundColor: this.disableBg,
                cursor: 'default'
            },
            buttonStyle: {
                height: this.height,
                width: this.width,
                borderRadius: "4px",
                fontSize: this.fontSize,
                color: this.color,
                border: "1px solid #e7e7e7",
                fontWeight: 700
            }
        }
    },

    computed: {
        startPage() {
            // When on the first page
            if (this.currentPage === 1) {
                return 1;
            }

            // When on the last page
            if (this.currentPage === this.totalPages) {
                return this.totalPages - this.maxVisibleButtons;
            }

            // When inbetween
            return this.currentPage - 1;
        },
        pages() {
            const range = [];

            for (let i = this.startPage; i <= Math.min(this.startPage + this.maxVisibleButtons - 1, this.totalPages); i++) {
                range.push({
                    name: i,
                    isDisabled: i === this.currentPage
                });
            }
            if (this.currentPage === this.totalPages) {
                for (let button = 0; button < this.maxVisibleButtons; button++) {
                    range[button].name = this.totalPages - (this.maxVisibleButtons - button - 1)
                }
            }
          
            return range;
        },
        isInFirstPage() {
            return this.currentPage === 1;
        },
        isInLastPage() {
            return this.currentPage === this.totalPages;
        },
    },
    methods: {
        isPageActive(page) {
            return this.currentPage === page;
        },
        onClickFirstPage() {
            this.$emit('pagechanged', 1);
        },
        onClickPreviousPage() {
            this.$emit('pagechanged', this.currentPage - 1);
        },
        onClickPage(page) {
            this.$emit('pagechanged', page);
        },
        onClickNextPage() {
            this.$emit('pagechanged', this.currentPage + 1);
        },
        onClickLastPage() {
            this.$emit('pagechanged', this.totalPages);
        }
    }
}
</script>
  
<style>
.pagination {
    list-style-type: none;
}

.pagination-item {
    display: inline-block;
}


.center {
    display: flex;
    list-style-type: none;
    flex-direction: row;
    align-items: center;
    justify-content: center;
}

button:hover {
    cursor: pointer
}

ul li {
    margin-left: 20px;
}

ul li:first-child ul li:last-child {
    margin-left: 0;
}

.left-arrow {
    transform: rotate(180deg);
}
</style>