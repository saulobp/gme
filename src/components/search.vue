<template>
    <div class="hello">
        <h1 style="margin-top:20px; color:darkblue;">Google ME</h1>
        <div class="container-fluid">
            <div class="pesquisa">
                <input type="text" class="form-control" v-model="query" @keyup.enter="search(1)"> <br>
                <button class="btn btn-primary" @click="search(1)">Buscar</button>
            </div>
        </div>
        <br><br>
        <div v-if="querySearch" class="container-fluid">
            <div v-for="item in items" :key="item.title" class="card">
                <!-- <a v-bind:href="item.link" target="_blank"> <b class="siteBaslik">{{item.title}}</b> </a> <br> -->
                <!-- <span>{{item.snippet}}</span> <br> -->
                <!-- <a v-bind:href="item.link" target="_blank"> {{item.link}}  </a> <br> -->
                <!-- <a v-bind:href="item.link" target="_blank"></a> <br> -->
                <div v-for="img in item.pagemap.cse_image" :key="img.src">
                    <a :href="item.link">
                        <img class="card-img-top" :src="img.src" :alt="item.title" target="_blank">
                    </a>

                    <!-- <p>{{img.src}}</p> -->
                </div>
            </div>
        </div>

        <div class="container-fluid">
            <ul class="pagination" v-if="querySearch">
                <li @click="search(1)">1</li>
                <li @click="search(2)">2</li>
                <li @click="search(3)">3</li>
                <li @click="search(4)">4</li>
                <li @click="search(5)">5</li>
                <li @click="search(6)">6</li>
                <li @click="search(7)">7</li>
                <li @click="search(8)">8</li>
                <li @click="search(9)">9</li>
                <li @click="search(10)">10</li>
            </ul>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        name: 'search',
        data() {
            return {
                items: null,
                itemsImg: null,
                query: '',
                querySearch: false,
                queryImgSearch: false,
            }
        },
        methods: {
            async search(start, img = undefined) {
                do {
                    let data = await axios.get('https://www.googleapis.com/customsearch/v1?key=AIzaSyBjIZb7Bse4fI5g8kbGEi4Evnru8d3yPeU&cx=2dc3eb65c5ee805e0&q=' + this.query + '&SearchType=image&ImgType=photo&start=' + start)
                        .then(res => {
                            console.log(res.data.items[0].pagemap.cse_image[0].src)
                            if (img) { start++ }
                            return res.data.items
                        })
                    this.queryImgSearch = false
                    this.querySearch = true
                    this.items = null
                    this.items = data
                }
                while (img && start < 5)
                scroll(0, 0)
            },
        }
    }
</script>


<style scoped>
    h1 {
        text-align: center;
    }

    .pagination {
        margin-top: 30px;
        width: 600px;
        display: block;
        margin-left: auto;
        margin-right: auto;
        text-align: center;
    }

        .pagination li {
            text-align: center;
            list-style-type: none;
            float: left;
            margin-right: 2%;
            margin-bottom: 20px;
            color: blue;
            cursor: pointer;
            width: 40px;
            height: 20px;
            /*background-color: lightgrey;*/
        }

            .pagination li:hover {
                text-decoration: underline;
            }

    .card {
        text-align: left;
        width: 200px;
        box-shadow: inset 0 0 1em gold, 0 0 1em black;
        margin: 15px;
        display: inline-block
    }

    .card-img-top {
        /*padding: 15px;*/
    }

    .pesquisa {
        margin-top: 30px;
        width: 600px;
        display: block;
        margin-left: auto;
        margin-right: auto;
        text-align: center;
    }

    input {
        border-radius: 15px;
    }
</style>
