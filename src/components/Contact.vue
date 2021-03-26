<template>
<div id="contact">
    <div class="section-title">
        <h4 class="dom-blue-text">Contact</h4>
    </div>
    <div class="container">
        <div class="section-header">
            <h5 class="dom-blue-text">Send Me a Message</h5>
        </div>
        <div class="grid">
            <div class="col-8">
                <p>Para
                </p>
            </div>
            <div class="col-4">
                <h6 class="dom-blue-text">- Contact Info -</h6>
                <p>dominicojeda.coding@gmail.com</p>
                <p>
                    <a href="https://www.linkedin.com/in/dominic-ojeda/" target="_blank"
                        class="contact-link">Linkedin</a>
                </p>
                <p>
                    <a href="https://github.com/dojeda1" target="_blank"
                        class="contact-link">Github</a>
                </p>
            </div>
        </div>
    </div>
</div>
</template>

<script>
export default {
    name: 'About',
    data() {
        return {
            title: 'My App is cooler than yours',
            isRotated: [
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false
            ],
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        rotate(ind) {
            var $this = this;
            if (!this.isRotated[ind]) {
                this.isRotated[ind] = true;
                setTimeout(function() {
                    $this.isRotated[ind] = false;
                },1000);
            }
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    .tool-logos {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        align-content: center;
    }
    .tool-logo {
        width: 50px;
        transition: all 1s ease-in-out;
    }
    .tool-logo:hover {
        transform: scale(1.1);
        transition: all .1s ease-in-out;
    }
    .interests {
        text-align: center;
    }
    .interest-icon {
        width: 80px;
        transition: all 1s ease-in-out;
        margin-top: 10px;
        margin-bottom: 15px;
    }
    .interest-icon:hover {
        transform: scale(1.1);
        transition: all .1s ease-in-out;
    }
    .contact-link {
        color: gray;
    }
    .contact-link:hover {
        color: #27aae1;
    }
</style>
