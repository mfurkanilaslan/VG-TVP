<h2>VG-TVP: Multimodal Procedural Planning via Visually Grounded Text-Video Prompting</h2>

<h3><a href="https://aaai.org/aaai-publications/aaai-conference-proceedings/"> VG-TVP Paper </a> :page_with_curl:</h3>
<h3><a href="https://drive.google.com/drive/folders/1-Lka5F-Dh-Fz6CwHDJYjUqieXlt2GCR6?usp=drive_link"> Daily-PP Dataset </a> :book: & :clapper: </h3>
<h3><a href="https://arxiv.org/abs/2412.11621"> Paper from Arxiv </a> :computer: </h3>
<h3><a href="https://twitter.com/muhammetfi"> Twitter </a> :bird: </h3>

<p>Thrilled to release the Visually Grounded Text-Video Prompting (VG-TVP) method which is a novel LLM-empowered Multimodal Procedural Planning (MPP) framework. It generates cohesive text and video procedural plans given a specified high-level objective. The main challenges are achieving textual and visual informativeness, temporal coherence, and accuracy in procedural plans. VG-TVP leverages the zero-shot reasoning capability of LLMs, the video-to-text generation ability of the video captioning models, and the text-to-video generation ability of diffusion models. VG-TVP improves the interaction between modalities by proposing a novel Fusion of Captioning (FoC) method and using Text-to-Video Bridge (T2V-B) and Video-to-Text Bridge (V2T-B). They allow LLMs to guide the generation of visually-grounded text plans and textual-grounded video plans. To address the scarcity of datasets suitable for MPP, we have curated a new dataset called Daily-Life Task Procedural Plans (Daily-PP). We conduct comprehensive experiments and benchmarks to evaluate human preferences (regarding textual and visual informativeness, temporal coherence, and plan accuracy). Our VG-TVP method outperforms unimodal baselines on the Daily-PP dataset.</p>
<p>Please check out our paper "VG-TVP: Multimodal Procedural Planning via Visually Grounded Text-Video Prompting"!</p>

<p>When you reach the dataset folder, you will see these subfiles.</p>
<ol>
  <li>All_Given_Videos_for_Dataset: Consists of all tasks' videos.</li>
  <li>Dataset: Consists of all tasks' details.</li>
  <li>Evaluations_and_Results: Consists of Automatic_Evaluations_All_Final_Results, LLM_Evaluations_Results, Video_CLIP_Scores_Randomly_Selected and Word_Counter_Results.</li> 
  <li>Figures_All: This file shows all figures used in the paper.</li>
  <li>Final_Paper: It consists of the final paper.</li>
  <li>Generated_Videos: It consists of all Seen and Unseen tasks' videos.</li>
  <li>Supplementary_Materials: This folder consists of supplementary materials that were submitted to the conference.</li>
  <li>Well_prepared_T2V_Generation_from_VG-TVP: Consists of well-prepared generated texts and relevant videos.</li>


<h3>Overview</h3>

<h4>VG-TVP </h4>
<p>In this paper, we aim to improve PP by generating multimodal content from different resources, including IVs. We are interested in enhancing human understanding by integrating visually grounded text and action-based video generations. We cast the problem as MPP via Visually Grounded Text-Video Prompting (VG-TVP). VG-TVP generates video-enhanced action and state procedures given text descriptions of a task and IVs, which is in contrast to generating image plans and using their descriptions as text plans.</p>

<img [Model_idea1.pdf](https://github.com/user-attachments/files/18196066/Model_idea1.pdf)>

<p>Figure 1: VG-TVP Model: Given the textual input and multiple instructional videos, VG-TVP generates visually grounded textual plans and video plans by using V2T-B and T2V-B. ChatGPT 3.5 is used to reorganize all captions to generate FoC.</p>
