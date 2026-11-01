```
<div class="actionButtons">
    <mwc-button
        raised
        id="primary-action-button"
        class="actionButton"
        slot="primaryAction"
        @click="submitAction">
        {{actionButton}}
    </mwc-button>
    <mwc-button
        raised
        slot="secondary-action-button"
        class="actionButton"
        @click="handleClear">
        Clear
    </mwc-button>
</div>
</div>
</div>
</template>
<script>
import '@material/mwc-textfield';
import '@material/mwc-button';
export default {
    name: 'Login',
    data() {
        return {
            actionText: 'Login',
            actionButton: 'Send',
            linkButton: 'Register'
        }
    },
    methods: {
        goToLink () {
            switch(this.linkButton) {
```