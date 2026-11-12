Plot the components of the dictionary learned from the patches. You will obtain a figure like the following screenshot:

![](bbox=[0.305, 0.163, 0.692, 0.453])

4. Define the show_with_diff() function and invoke it to show the distorted image:

```
def show_with_diff(image, reference, title):
plt.figure(figsize=(10, 5))
plt.subplot(121, plt.title('Image'))
plt.imshow(image, vmin=0, vmax=1, cmap=plt.cm.gray, \
interpolation='nearest'), plt.axis('off')

plt.subplot(122)
difference = image - reference
plt.title('Difference (norm: %.2f)' % \
np.sqrt(np.sum(difference ** 2)))
plt.imshow(difference, vmin=-0.5, vmax=0.5, cmap=plt.cm.gray_r, \
interpolation='nearest')
plt.axis('off'), plt.suptitle(title, size=20)
plt.subplots_adjust(0.02, 0.02, 0.98, 0.79, 0.02, 0.2)
plt.show()

show_with_diff(distorted, lena, 'Distorted image')
```