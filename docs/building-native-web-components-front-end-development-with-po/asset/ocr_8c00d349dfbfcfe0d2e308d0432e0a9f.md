```
id="text-description"
outlined
minlength="3"
label="Description"
required>
</mwc-textfield>
</div>
<div>
<mwc-button
id="primary-action-button"
slot="primaryAction"
@click="handleAddNote">
Add
</mwc-button>
<mwc-button
slot="secondaryAction"
dialogAction="close"
@click="handleClose">
Cancel
</mwc-button>
</div>
</mwc-dialog>
</div>
</template>
<script>
import '@material/mwc-list/mwc-list';
import '@material/mwc-list/mwc-list-item';
import '@material/mwc-fab';
import '@material/mwc-button';
import '@material/mwc-dialog';
import '@material/mwc-textfield';
import { notesData } from '../utils/DummyData';
import { v4 as uuidv4 } from 'uuid';
```