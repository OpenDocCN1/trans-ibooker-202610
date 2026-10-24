
high-income households within a certain radius does not correspond to potential attendance; it serves as a proxy to represent a certain “social atmosphere” that is more or less appreciated by each household according to its income. In this way, the effect of belonging to the same territory and not that of friction of distance is modeled. The same logic applies to the assessment of the proportion of wood and forest areas and built areas in a certain neighborhood.

![](img/d2fb8631c02882e36689e61a57b94123_78_0.png)

Figure 3.3. Rules for assessing the elements composing the local residential environment of a household in MobiSim-MR – parameter setting of the rules for application to Besançon’s metropolitan area. The value of 1 on the y-axis corresponds to a maximum appreciation. Based on (Tannier et al. 2016b). Note: distances are calculated as the crow flies

The other two models we are interested in are not multi-agent models. These are MUP-City, which makes it possible to simulate fractal residential development under accessibility constraints (Tannier et al. 2012), and Fractalopolis, dedicated to the interactive design of multi-fractal urban development plans (Frankhauser et al. 2018). The purpose of these two models is to simulate different possible urban development configurations based on an initial spatial configuration (buildings, road and path networks, public transport stations, shops and services, and green and recreational infrastructures). They do not integrate any temporal dynamics: not only is the configuration obtained by simulation not located in time but also the transition from the initial configuration to the simulated configuration is done in a single step (“one-shot”) and is not the result of a trajectory that would have passed successively through a series of intermediate states.

The simulation results are potentially interesting cells to urbanize in the case of MUP-City, and urban or suburban centers in which new housings could be built in the case of Fractalopolis. The rules governing the two models are of a geometric nature. In the case of MUP-City, it is a fractal rule of urbanization that imposes particular spacing between built cells and a certain form of connectivity between undeveloped cells. In the case of Fractalopolis, it is a system of iterative functions (Barnsley 1988) that jointly define the spacing between the different centers (i.e. their relative position) and their surface, as well as a certain connectivity of the spaces located outside the centers. With these geometric rules, distances between spatial entities created by simulation (cells or centers) are imposed without modeling the effects of distance in terms of neither friction nor membership to the same territory. These effects are indirectly taken into account through a series of hypotheses whereby it is assumed that the spacing between spatial entities (cells or centers) imposed by geometric rules allows certain mobility behaviors that are desired for a given development scenario, while they prevent others that are avoided for that scenario. The mobility behaviors concerned are the movement of individuals to access shops and services, public transports, and green and leisure infrastructures, as well as the juvenile dispersion of animal populations between different ecological areas (Frankhauser et al. 2018; Tannier et al. 2016a).

In addition to the geometric rules for locating entities created by simulation (cells or centers), the effects of distance in terms of friction and the role of belonging to the same territory are directly modeled as part of the assessment of the interest of cells or centers in residential development (Table 3.1). To this end, the MUP-City and Fractalopolis models incorporate spatial accessibility measures, just like the SimFeodal and MobiSim-MR models: distance to the nearest opportunity/opportunities (points of interest) and number of opportunities in a certain neighborhood. However, unlike SimFeodal and MobiSim-MR, effects other than those of distance are also modeled in MUP-City and Fractalopolis, in order to represent the mobility behaviors of individuals in more detail. In particular, the diversity of the supply of shops and services, green and leisure infrastructures, and public transport are taken into account (Figure 3.5 and Table 3.2).

![](img/d2fb8631c02882e36689e61a57b94123_80_0.png)

Figure 3.4. Rules for assessing the elements composing the overall residential environment of a household in MobiSim-MR. The value of 1 corresponds to a maximum appreciation (Tannier et al. 2016b)

In Fractalopolis, each planned development center has a neighborhood (i.e. a catchment area) of which the size varies according to the distance between the center concerned and neighboring centers of higher functional level. The size of the neighborhood (catchment area) of centers of the same hierarchical level may be different, while the size of the neighborhood (catchment area) of centers of different hierarchical levels may be identical.

Commercial cluster frequented daily

![](img/d2fb8631c02882e36689e61a57b94123_81_0.png)

Commercial cluster frequented weekly

![](img/d2fb8631c02882e36689e61a57b94123_81_1.png)

largest catchment area of an establishment in the study area (default value: 4000 m)

Commercial cluster frequented monthly or more rarely

Evaluation of the distance to the *m* closest establishments of each type

![](img/d2fb8631c02882e36689e61a57b94123_81_2.png)

Access to public transport

![](img/d2fb8631c02882e36689e61a57b94123_81_3.png)

**Figure 3.5.** MUP-City 1.2: accessibility variables used in the model to evaluate the interest of cells to be urbanized (default setting values). The value of 1 on the y-axis corresponds to a maximum appreciation

|  | Number of opportunities from which residents’ satisfaction is highest | Number of different types of opportunities from which residents’ satisfaction is highest |
|---|---:|---:|
| Shops and services used daily | 8 | 5 |
| Shops and services used weekly | 10 | 7 |
| Shops and services used monthly | *Not considered* | 2 |
| Green and recreational infrastructures used daily | *Not considered* | 2 |
| Green and recreational infrastructures used weekly | *Not considered* | 10 |
| Green and leisure infrastructures used monthly | *Not considered* | 8 |

**Table 3.2.** *Fractalopolis 1.0: configuration of accessibility rules for the design of urban development scenarios for Besançon’s metropolitan area (France)*

In MUP-City, neighborhoods are determined by distance measured on the road and path network or as the crow flies (at the choice of the model user) (Figure 3.5). In the case, for example, of shops and services with daily and weekly use, the nearest establishment must be located at a maximum distance δₘₐₓ (default values: 600 m for daily use and 2,000 m for weekly use) from the residential cell to be assessed. Otherwise, the evaluation drops to 0. From the nearest establishment located at less than 600 m, all establishments located at a maximum distance of 200 m from each other are considered, within the limit of a distance δₘₐₓₘₐₓ between the most distant establishment and the cell to be evaluated (default values: 1,000 m for daily attendance and 3,000 m for weekly attendance). In doing so, each commercial cluster is specific to each residential cell. The interest of a cluster of shops and services considering a given cell corresponds to the evaluation of the number and diversity of establishments present in this cluster combined with the distance that separates this cluster from the evaluated cell. When this distance is large (close to δₘₐₓ), a good assessment of the characteristics of the cluster (high number of establishments and wide diversity of types of establishments present) does not compensate for the poor assessment of the distance between the cell assessed and this cluster. On the contrary, when the distance between the cluster and the evaluated cell is close to 0, a poor evaluation of the characteristics of the cluster is compensated by the good evaluation of the distance from the cell to this cluster.

Unlike daily and weekly shops and services, where it is interesting to have a substantial supply in a very close neighborhood, it can be considered that having a shop or service of each category in “reasonable” proximity to the cell is sufficient in the case of monthly or less frequent use. For example, it is not necessary to have two hospitals, two dentists, or two libraries near your home. On the other hand, it is of interest to be within a few tens of minutes of all these services. This hypothesis is also justified in terms of accessibility to public administrations: it is not necessary to be equidistant from two municipal administrations, since by definition, a dwelling is located in a single municipality.

Finally, with regard to the evaluation of accessibility to green and leisure infrastructures, the hypothesis is that the diversity of infrastructures present in the vicinity of a cell is more important than the number of infrastructures. Thus, access to a single football field or tennis court is as interesting as access to two of these fields. On the other hand, access to a football field and a tennis court is more valued than access to only one type of field.

If we now compare the parameter setting of the accessibility rules made for the application of Fractalopolis and MUP-City to the same study area, i.e. the Besançon metropolitan area, we also notice differences (see Figure 3.5 and Table 3.2). In particular, the number of shops and services used daily from which total resident satisfaction is the maximum is equal to 3 in the case of the MUP-City application and 8 in the case of the Fractalopolis application. For shops and services that are used weekly, the same numbers are 15 and 10, respectively. These differences in values are mainly due to the different definition of cell neighborhoods (MUP-City) and development centers (Fractalopolis), as well as the use of different data for the two applications. It should be noted that, for the application of Fractalopolis, only two types of shops and services with monthly attendance were distinguished (hospitals and large shopping centers). This explains why two different types of opportunities are sufficient for the maximum resident satisfaction.
