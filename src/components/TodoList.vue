<template>
    <main id="todolist">

        <h1>Todo List <span>Get things done, one item at a time.</span></h1>
        <div class="items" v-if="items.length">

            <ul>

                <li v-for="(item, index) in items" :key="index" :class="{'done' : item.done}">
                    <span class="label">{{item.title}}</span>

                    <div class="actions">

                        <button class="btn-picto" type="button" @click="changeItemStatus(index)">
                            <i aria-hidden="true" class="material-icons">{{item.done ? 'check_box' :
                                'check_box_outline_blank'}}</i>
                        </button>

                        <button class="btn-picto" type="button"
                                aria-label="Delete" title="Delete" @click="deleteItem(index)">
                            <i aria-hidden="true" class="material-icons">delete</i>
                        </button>

                    </div>

                </li>
            </ul>
        </div>

        <p class="emptylist" v-else>Your todo list is empty.</p>

        <form @submit.prevent="addItem">
            <label for="newitem">Add to the todo list</label>
            <input type="text" name="newitem" id="newitem" v-model="itemTitle">
            <button type="submit">Add item</button>
        </form>

    </main>
    
</template>

<script>
    const localKey = 'todos';
    export default {
        name: 'TodoList',
  props: {
    msg: String
  },
        data() {
            return {
                itemTitle: '',
                items: [],
            }
        },
        methods: {
            addItem() {
                if (!this.itemTitle) {
                    return;
                }

                this.items.push({
                    title: this.itemTitle,
                    done: false,
                });

                this.itemTitle = '';
            },
            deleteItem(index) {
                this.items.splice(index, 1);
            },
            changeItemStatus(index) {
                const item = this.items[index];
                this.items[index].done = !item.done;
            }
        },
        mounted() {
            const items = localStorage.getItem(localKey) || '[]';
            this.items = JSON.parse(items);
        },
        watch: {
            items: {
                deep: true,
                handler(items) {
                    localStorage.setItem(localKey, JSON.stringify(items))

                }
            }
        }
    }
</script>

<style>

/* ================ STYLE GLOBAL ================*/
    * {
        margin: 0;
        padding: 0;
        outline: none;
        box-sizing: border-box;
    }

    html, body {
        background: #f7f1f1;
        font-size: 1.1rem;
        font-family: 'Quicksand', sans-serif;
        height: 100%;
    }

:root{
    --pink: #FF6666;
    --light-pink:#FF5E5E;
    --white:#fff;
    --light-black:rgba(100, 100, 100, .1);
}

/* ================ END GLOBAL ================*/

/* ================ STYLE MAIN ================*/

    @keyframes strikeitem {
        to {
            width: calc(100% + 1rem);
        }
    }


/* ================ STYLE TODOLIST ================*/

    #todolist {
        margin: 4rem auto;
        padding: 2rem 3rem 3rem;
        max-width: 500px;
        background: var(--pink);
        color:var(--white);
        box-shadow: -20px -20px 0px 0px var(--light-black);
    }

    #todolist h1 {
        /*text-align:center;*/
        font-weight: normal;
        font-size: 2.6rem;
        letter-spacing: 0.05em;
        border-bottom: 1px solid var(--light-black);
    }

    #todolist h1 span {
        display: block;
        font-size: 0.8rem;
        margin-bottom: 0.7rem;
        margin-left: 3px;
        margin-top: 0.2rem;
    }

    #todolist .emptylist {
        margin-top: 2.6rem;
        text-align: center;
        letter-spacing: .05em;
        font-style: italic;
        opacity: 0.8;

    }

    #todolist ul {
        margin-top: 2.6rem;
        list-style: none;
    }

    #todolist .todolist-move {
        transition: transform 1s;
    }

    #todolist li {
        display: flex;
        margin: 0 -3rem 4px;
        padding: 1.1rem 3rem;
        justify-content: space-between;
        align-items: center;
        background: rgba(255, 255, 255, 0.1);
    }

    #todolist .actions {
        flex-shrink: 0;
        padding-left: 0.7em;
    }

    #todolist .label {
        position: relative;
        transition: opacity .2s linear;
    }

    #todolist .done .label {
        opacity: .6;
    }

    #todolist .done .label:before {
        content: '';
        position: absolute;
        top: 50%;
        left: -.5rem;
        display: block;
        width: 0%;
        height: 1px;
        background: var(--white);
        animation: strikeitem .3s ease-out 0s forwards;
    }

    #todolist .btn-picto {
        border: none;
        background: none;
        -webkit-appearance: none;
        cursor: pointer;
        color: var(--white);
    }


   /* ================ STYLE FORM ================*/

    form {
        margin-top: 3rem;
        display: flex;
        flex-wrap: wrap;
    }

    form label {
        min-width: 100%;
        margin-bottom: .5rem;
        font-size: 1.3rem;
    }

    form input {
        flex-grow: 1;
        border: none;
        background: #f7f1f1;
        padding: 0 1.5em;
        font-size: initial;
    }

    form button {
        padding: 0 1.3rem;
        border: none;
        background:var(--pink);
        color: white;
        text-transform: uppercase;
        font-weight: bold;
        border: 1px solid rgba(255, 255, 255, .3);
        margin-left: 5px;
        cursor: pointer;
        transition: background .2s ease-out;
    }

    form button:hover {
        background:var(--light-pink);
    }

    form input,
    form button {
        font-family: 'Quicksand', sans-serif;
        height: 3rem;
    }

    /* ================ END MAIN ================*/
</style>