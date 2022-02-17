---
layout: page
title: Siléane Dataset
permalink: /dataset2017.html
---


<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

<h1><em>Siléane</em> Dataset  for Object Detection and Pose Estimation</h1>
    
<p>This dataset consists in a total of 2601 independent scenes depicting various numbers of object instances in bulk, fully annotated. It is primarily designed for the evaluation of object detection and pose estimation methods based on depth or RGBD data, and consists of both synthetic and real data.</p>

<p>Synthetic data is made of scenes representing the inside of a bin, in which a random number of object instances have been dropped (in some scenes, there are no visible instances at all).</p>
<figure>
<img src="/assets/iccvw2017/bunny_3_080/bunny_3_080.jpg" style='max-width:32%' alt="bunny" /><img src="/assets/iccvw2017/bunny_3_080/bunny_3_080_depth_gt_colored.jpg" style='max-width:32%' alt="bunny" /><img src="/assets/iccvw2017/bunny_3_080/bunny_3_080_depth_colored.jpg" style='max-width:32%' alt="bunny" /> 
<figcaption>Sample scene from the synthetic <em>bunny</em> dataset (RGB, ideal and noisy depth image).</figcaption>
</figure>


<p>Real data consists similarly in scenes of various numbers of object instances (between 0 and 11) lying on a surface at various distances from the camera.
We considered two different background surfaces: a planar one (<em>markers flat</em>, 308 scenes), representative of the typical bottom of a bin; and a bumpy surface (<em>markers bump</em>, 325 scenes), increasing the variability of poses and producing a pose distribution more consistent with the scenario of many instances piled up.
In order to validate the use of synthetic data for evaluation, we also propose a synthetic dataset (<em>markers flat simulation</em>) reproducing real scenes that enables pairwise comparison.</p>

<figure>
    <figure>
    <img src="/assets/iccvw2017/2016_10_10_plat_h2_10_019_intensity_cropped.jpg" style='max-width:49%' alt="markers flat" /> 
    <img src="/assets/iccvw2017/2016_10_10_plat_h2_10_019_depth_colored_cropped.jpg" style='max-width:49%' alt="markers flat" /> 
    <figcaption>Sample scene from the real <em>markers flat</em> dataset (RGB and depth image).
    </figcaption>
    </figure>
    <figure>
    <img src="/assets/iccvw2017/2016_10_10_plat_h2_10_019_intensity_simulation_cropped.jpg" style='max-width:49%' alt="markers flat" /> 
    <img src="/assets/iccvw2017/2016_10_10_plat_h2_10_019_depth_simulation_colored_cropped.jpg" style='max-width:49%' alt="markers flat" /> 
    <figcaption>Corresponding scene in the synthetic <em> markers flat simulation</em> dataset. </figcaption>
    </figure>
</figure>

<p>Lastly, an additional dataset of 46 scenes (<em>markers clutter</em>) targets the problem of object detection and pose estimation in a cluttered environment.</p>
    

<h2>Dataset description</h2>
    
<h3> Archive description</h3>

- <em>rgb</em>: RGB or intensity image.
- <em>depth</em>: depth image from the same viewpoint as the RGB data, encoded as a 16bit unsigned PNG image.
- <em>depth_gt</em>: ideal depth image, if available.
- <em>segmentation</em>: segmentation of the various instances of the scene.
- <em>gt</em>: ground truth annotations for each scene.
- <em>mesh.ply</em>: 3D triangular mesh model of the object.
- <em>poseutils.json</em>: description of geometric properties of the object used by evaluation scripts, notably its symmetry group.
- <em>camera_params.txt</em>: camera parameters considered.

<h3> File format </h3>

<h4> Ground truth </h4>
<p>Ground truth pose annotations for a dataset are provided in JSON format for each scene such as follows:</p>
```
[{"R": [[-0.71494568144, -0.29784533696000004, -0.63256695358],
 [0.65633897998, -0.5977253336480001, -0.46037342646400004],
 [-0.24098120024000003, -0.7443202964360001, 0.622828612352]],
"segmentation_id": 65535,
"occlusion_rate": 0.10165770117466244,
"t": [-0.133393, -0.043768, 0.221253]},

{"R": [[-0.160880605701, 0.10835646816000002, 0.9810084635799999],
 [0.40743837264000005, -0.8980182051009999, 0.16600775556000003],
 [0.8989508898199999, 0.42640764324, 0.10032506454899998]],
 "segmentation_id": 64599,
 "occlusion_rate": 0.09766150740242263, "t": [-0.132352, 0.177468, 0.233303]}]
```
<p>In this example, the scene contains two object instances. Their poses are described by a rotation matrix \(R\) and a translation vector \(t\) in such a way that a point \(x\) expressed in the object frame can be expressed in the absolute coordinates system by \(R x + t\). Occlusion rate of each instance is also provided (defined as the fraction of the object's silhouette visible in the depth data), as well as the color in the 16bit segmentation image.</p>

<h4>Camera parameters</h4>
Camera parameters are described within a text file of the following format:
```
width	506
height	474
fu	1152.31
fv	1152.31
cu	139.459
cv	237.001
clip_start	0.87599
clip_end	1.70602
# Location of the camera, expressed by a 3D translation vector.
location	-0.1	0	1.5
# Orientation of the camera, expressed by an unit quaternion (w x y z).
rotation	8.72098e-008	1	-4.57187e-009	-9.94784e-010
```
<p>
The coordinates \((x, y, z)\) of in camera frame can be computed  from the value of the depth image \(D(u, v) \in [0, 1] \) at a pixel \((u, v)\) as follows:
\[
\left\lbrace
\begin{align}
z &= \mathrm{clip\_start} + (\mathrm{clip\_end} - \mathrm{clip\_start})D(u, v) \\
x &= \cfrac{z}{f_u} (u - c_u)   \\
y &= \cfrac{z}{f_v} (v - c_v).
\end{align}\right.
\]
White pixels in the depth image represent points which have not been reconstructed.
</p>

<h3>Symmetry group</h3>

<p>The proposed evaluation methodology is based on the symmetry group of the object considered .The symmetry group depends on what static configurations of the object we wish to distinguish, and this choice is not necessarily obvious. 
For example, the <em>gear</em> object could be considered as an object with a cyclic symmetry of order 2 -- <em>i.e.</em> an invariance under rotation of 1/2 turn about a given axis -- a cyclic symmetry of order its number of teeth, or a revolution symmetry depending on the level of details considered. We considered this latter option in our experiments, and synthesize the choices of symmetry classes we made below:</p>

<figure><img src="/assets/iccvw2017/objects.PNG" style='max-width: 500px;' alt="objects" /></figure>

<h3>3D models</h3>
<p>3D models of the <em>tless</em> objects used in this dataset are taken from the <a href="http://cmp.felk.cvut.cz/t-less/">T-LESS</a> dataset of T. Hodaň et al. The <em>bunny</em> and <em>markers</em> models are modified versions of original Stanford bunny from the <a href="http://graphics.stanford.edu/data/3Dscanrep/">Stanford University Computer Graphics Laboratory</a>. Other models have been downloaded from online archives in 2016-2017: "Pepper & Salt Mill Peugeot" by Ramenta (3D Warehouse), "Samdan 2" by Metin N. (3D Warehouse).</p>


<h2>Download</h2>

<p>The data can be downloaded as a set of 7zip archives <a href="https://drive.google.com/drive/folders/0B2h14hEw1jRRRGJ5WGFockI5a3M?resourcekey=0-DtEuXPQ_Jem1l6AC2_iQUA&usp=sharing">here</a>.
</p>


<p>Additionally, we provide some evaluation tools on GitHub to ease the use of this dataset: <a href="https://github.com/rbregier/pose_recovery_evaluation">GitHub link</a>.</p>

<p>Please cite the following paper if you use this dataset:<br/>
<cite>Romain Brégier, Frédéric Devernay, Laetitia Leyrit and James L. Crowley, "Symmetry Aware Evaluation of 3D Object Detection and Pose Estimation in Scenes of Many Parts in Bulk", in <em>The IEEE International Conference on Computer Vision (ICCV)</em>, 2017, pp. 2209-2218.</cite>
(<a href="http://openaccess.thecvf.com/content_ICCV_2017_workshops/papers/w31/Bregier_Symmetry_Aware_Evaluation_ICCV_2017_paper.pdf">paper</a>, 
<a href="http://openaccess.thecvf.com/content_ICCV_2017_workshops/supplemental/Bregier_Symmetry_Aware_Evaluation_ICCV_2017_supplemental.pdf">supplementary material </a>,
<a href="/assets/iccvw2017/rbregier_iccvw_2017_poster.pdf">poster</a>).</p>
```
@InProceedings{bregier2017iccv,
author = {Br{\'e}gier, Romain and Devernay, Fr{\'e}d{\'e}ric and Leyrit, Laetitia and Crowley, James L.},
title = {Symmetry Aware Evaluation of 3D Object Detection and Pose Estimation in Scenes of Many Parts in Bulk},
booktitle = {The IEEE International Conference on Computer Vision (ICCV)},
month = {Oct},
year = {2017}
}
```

<p>For any question or remark, contact information can be found <a href="index.html">here</a>.</p>


The Siléane Dataset is licensed under the <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.

<a href="http://www.sileane.com/en/"><img alt="sileane.com" src="/assets/iccvw2017/sileane.png"/></a>