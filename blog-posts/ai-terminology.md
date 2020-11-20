## AI terminology

1. Symbolic AI: AI system in which problem solving is achieved by a (very) large set of hardcoded rules crafted by programmers. Symbolic AI is suitable only for well-defined, logical problems such as playing chess-like games. It is NOT suitable for more complex problems such as image/speech recognition.

2. Machine learning: a different paradigm for AI development. Symbolic AI paradigm is that programmers input rules and data to be processed by these rules then the computer will outcome an answer. The machine learning paradigm, on the other hand, relies on the so-called **training** process in which a programmer provides a machine with data (training data) and an answer for the problem at hand, then the machine will outcome a set of rules, or in machine learning language, a **model**. The training process will be coupled with a **testing** process in which the obtained model will be re-evaluated on a different set of data.

3. Data representation: different ways to look at a dataset. A picture can be encoded with RGB (red-green-blue) format or HSV (hue-saturation-value) format. These are two different repesentations of the same data. Some representation will be better than others for a specific problem. For example, if you want to select all red pixels in a picture, using RGB format will definitely a better choice, but if you want to make an image less saturated, using HSV will be the way to go.

4. Learning: is an automatic process to search for better representations of a dataset.

5. Deep learning: a subfield of machine learning in which emphasis is placed on building data representations that are heavily layered. The example we see above about the representation of a colored image is an exceedingly simple one. A more realistic representation of a picture is usually understood as human-level abstractions like meaning and context. These high-level abstractions are, in their turn, based on object identification, inter-relationships between objects like intra-class variation, scaling variation, occlusion, relative positions and angles, and so on. At an even deeper level, these specification of the objects and their visual relationships depends on low-level ingredients like shapes and illumination. At the bottom of this abstraction hierarchy is the raw data, like RGB or HSV pixel map mentioned above. The deep learning exploits the successive layers of increasingly meaningful representation of data.


