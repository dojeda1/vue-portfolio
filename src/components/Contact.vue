<template>
<div id="contact">
    <div class="section-title">
        <h4 class="text-blue">Contact</h4>
    </div>
    <div class="container">
        <div class="section-header">
            <h5 class="text-blue">Send Me a Message</h5>
        </div>
        <div class="grid">
            <div class="col-8 s_col-12 dual-buttons">
                <a target="_blank" href="mailto:dominicojeda.coding@gmail.com">
                    <button class="btn-blue">Email<i className="material-icons">send</i></button>
                </a>
            </div>
            <div class="col-4 s_col-12">
                <h6 class="text-blue">- Contact Info -</h6>
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
            const $this = this;
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
    .contact-link {
        color: gray;
    }
    .contact-link:hover {
        color: #27aae1;
    }
</style>
