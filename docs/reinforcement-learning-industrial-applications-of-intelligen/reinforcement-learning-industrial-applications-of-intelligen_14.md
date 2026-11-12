# 附录 B. Softmax 策略的梯度

Softmax 策略梯度的推导见 公式 B-1。请注意这与 附录 A 中的逻辑梯度具有相似的形式。

## 公式 B-1. Softmax 策略的梯度

$$\\begin{array}{cl} {\\nabla\_{\\theta}\\ln\\pi\\left( \\theta^{\\intercal}s \\right)} & {= \\nabla\_{\\theta}\\ln\\frac{e^{\\theta^{\\intercal}}s}{\\sum\_{a}e^{\\theta\_{a}^{\\intercal}s}}} \\\\ & {= \\nabla\_{\\theta}\\ln e^{\\theta^{\\intercal}}s - \\nabla\_{\\theta}\\ln\\sum\\limits\_{a}e^{\\theta\_{a}^{\\intercal}s}} \\\\ & {= \\nabla\_{\\theta}\\theta^{\\intercal}s - \\nabla\_{\\theta}\\ln\\sum\\limits\_{a}e^{\\theta\_{a}^{\\intercal}s}} \\\\ & {= s - \\nabla\_{\\theta}\\ln\\sum\\limits\_{a}e^{\\theta\_{a}^{\\intercal}s}} \\\\ & {= s - \\nabla\_{\\theta}\\ln\\sum\\limits\_{a}e^{\\theta\_{a}^{\\intercal}s}} \\\\ & {= s - \\frac{\\nabla\_{\\theta}\\sum\_{a}e^{\\theta\_{a}^{\\intercal}s}}{\\sum\_{a}e^{\\theta\_{a}^{\\intercal}s}}} \\\\ & {= s - \\frac{\\sum\_{a}se^{\\theta\_{a}^{\\intercal}s}}{\\sum\_{a}e^{\\theta\_{a}^{\\intercal}s}}} \\\\ & {= s - \\sum\\limits\_{a}s\\pi\\left( \\theta^{\\intercal}s \\right)} \\\\ \\end{array}$$
