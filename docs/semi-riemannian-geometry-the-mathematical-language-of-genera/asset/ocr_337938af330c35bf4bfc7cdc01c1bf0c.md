Proof. We have from (18.7.1) and (18.7.2) that

$$\sum_{l} R^{l}_{ijk}\,\frac{\partial}{\partial x^{l}} = R\left(\frac{\partial}{\partial x^{i}},\frac{\partial}{\partial x^{j}}\right)\frac{\partial}{\partial x^{k}}$$

$$= \nabla_{\partial/\partial x^{i}}\left(\nabla_{\partial/\partial x^{j}}\left(\frac{\partial}{\partial x^{k}}\right)\right) - \nabla_{\partial/\partial x^{j}}\left(\nabla_{\partial/\partial x^{i}}\left(\frac{\partial}{\partial x^{k}}\right)\right)$$ 

$$- \nabla_{[\partial/\partial x^{i},\partial/\partial x^{j}]}\left(\frac{\partial}{\partial x^{k}}\right).$$

Then (18.2.1) gives

$$\nabla_{\partial/\partial x^{i}}\left(\nabla_{\partial/\partial x^{j}}\left(\frac{\partial}{\partial x^{k}}\right)\right) = \nabla_{\partial/\partial x^{i}}\left(\sum_{l}\Gamma^{l}_{jk}\frac{\partial}{\partial x^{l}}\right) = \sum_{l}\nabla_{\partial/\partial x^{i}}\left(\Gamma^{l}_{jk}\frac{\partial}{\partial x^{l}}\right)$$ 

$$= \sum_{l}\left[\frac{\partial\Gamma^{l}_{jk}}{\partial x^{i}}\frac{\partial}{\partial x^{l}} + \Gamma^{l}_{jk}\nabla_{\partial/\partial x^{i}}\left(\frac{\partial}{\partial x^{l}}\right)\right]$$

$$= \sum_{l}\left(\frac{\partial\Gamma^{l}_{jk}}{\partial x^{i}}\frac{\partial}{\partial x^{l}} + \Gamma^{l}_{jk}\sum_{n}\Gamma^{n}_{il}\frac{\partial}{\partial x^{n}}\right)$$ 

$$= \sum_{l}\frac{\partial\Gamma^{l}_{jk}}{\partial x^{i}}\frac{\partial}{\partial x^{l}} + \sum_{ln}\Gamma^{l}_{jk}\Gamma^{n}_{il}\frac{\partial}{\partial x^{n}}$$

$$= \sum_{l}\frac{\partial\Gamma^{l}_{jk}}{\partial x^{i}}\frac{\partial}{\partial x^{l}} + \sum_{ln}\Gamma^{n}_{jk}\Gamma^{l}_{in}\frac{\partial}{\partial x^{l}}$$

$$= \left(\sum_{l}\frac{\partial\Gamma^{l}_{jk}}{\partial x^{i}} + \sum_{ln}\Gamma^{n}_{jk}\Gamma^{l}_{in}\right)\frac{\partial}{\partial x^{l}}$$

and

$$\nabla_{\partial/\partial x^{j}}\left(\nabla_{\partial/\partial x^{i}}\left(\frac{\partial}{\partial x^{k}}\right)\right) = \left(\sum_{l}\frac{\partial\Gamma^{l}_{ik}}{\partial x^{j}} + \sum_{ln}\Gamma^{n}_{ik}\Gamma^{l}_{jn}\right)\frac{\partial}{\partial x^{l}},$$

and Theorem 15.3.3(b) yields

$$\nabla_{[\partial/\partial x^{i},\partial/\partial x^{j}]}\left(\frac{\partial}{\partial x^{k}}\right) = 0.$$

Combining the above identities gives the result.

In the literature, the indexing of $R^{l}_{ijk}$ is sometimes defined by

$$R\left(\frac{\partial}{\partial x^{j}},\frac{\partial}{\partial x^{k}}\right)\frac{\partial}{\partial x^{i}} = \sum_{l} R^{l}_{ijk}\,\frac{\partial}{\partial x^{l}}.$$

This changes the indexing of certain identities, but has no substantive implications.