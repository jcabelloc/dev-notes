# Recurrent python tips

``` python
# type of an object
type(data_object)


```

## Manipulating images

### Loading an image
```python
img_path = 'C:/Users/jcabelloc/Downloads/download.jpg'
# We preprocess the image into a 4D tensor
from keras.preprocessing import image
import numpy as np

img = image.load_img(img_path, target_size=(150, 300))
img_tensor = image.img_to_array(img)
img_tensor = np.expand_dims(img_tensor, axis=0)
# Remember that the model was trained on inputs
# that were preprocessed in the following way:
img_tensor /= 255.
# Its shape is (1, 150, 300, 3)
print(img_tensor.shape)

```

### Showing an image
```python
import matplotlib.pyplot as plt
plt.imshow(img_tensor[0])
plt.show()
```