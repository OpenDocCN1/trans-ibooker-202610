# Listing 6.9 Defining the multi-class classifier

```
class MultilabelClassifier(nn.Module):
    def __init__(self, n_brand, n_vehicle_type, n_cohort):
        super().__init__()
        self.resnet = models.resnet34(pretrained=True) #A
        self.model_wo_fc = nn.Sequential(*(list(self.resnet.children())[:-1])) #B
        self.brand = nn.Sequential( #C
            nn.Dropout(p=0.2),
            nn.Linear(in_features=512, out_features=n_brand)
        )
        self.vehicle_type = nn.Sequential( #D
            nn.Dropout(p=0.2),
            nn.Linear(in_features=512, out_features=n_vehicle_type)
        )
        self.cohort = nn.Sequential( #E
            nn.Dropout(p=0.2),
            nn.Linear(in_features=512, out_features=n_cohort)
        )

    def forward(self, x): #F
        x = self.model_wo_fc(x)
        x = torch.flatten(x, 1)
        return {
            'brand': self.brand(x),
            'vehicle_type': self.vehicle_type(x),
            'cohort': self.cohort(x)
        }
```

#A download the backbone architecture
#B remove the last FC layer
#C define the branching head for brand classification
#D define the branching head for vehicle type classification
#E define the branching head for cohort classification
#F define the flow of the model

As shown in the following code snippet, we can then instantiate a model object based on the pre-determined number of categories for each target label and move it to GPU for accelerated training if available.

```
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
model = MultilabelClassifier(6, 5, 2).to(device)
```

Figure 6.20 visualizes the overall architecture of the model. We use the resnet34 (after removing the final FC layer) as the backbone shared across all three tasks. The final output of the resnet34 has 512 nodes, which branches into three separate classification heads, all consisting of fully-connected linear layers.