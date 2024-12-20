<h1>VG-TVP: Multimodal Procedural Planning via Visually Grounded Text-Video Prompting</h1>

<h3><a href="https://aaai.org/aaai-publications/aaai-conference-proceedings/"> VG-TVP Paper </a> :page_with_curl:</h3> 
<h3><a href="https://drive.google.com/drive/folders/1-Lka5F-Dh-Fz6CwHDJYjUqieXlt2GCR6?usp=drive_link"> Daily-PP Dataset </a> :book: & :clapper: </h3> 
<h3><a href="https://arxiv.org/abs/2412.11621"> Paper from Arxiv </a> :computer: </h3> 
<h3><a href="https://twitter.com/muhammetfi"> Twitter </a> :bird: </h3>

<p>Thrilled to release the Visually Grounded Text-Video Prompting (VG-TVP) method which is a novel LLM-empowered Multimodal Procedural Planning (MPP) framework. It generates cohesive text and video procedural plans given a specified high-level objective. The main challenges are achieving textual and visual informativeness, temporal coherence, and accuracy in procedural plans. VG-TVP leverages the zero-shot reasoning capability of LLMs, the video-to-text generation ability of the video captioning models, and the text-to-video generation ability of diffusion models. VG-TVP improves the interaction between modalities by proposing a novel Fusion of Captioning (FoC) method and using Text-to-Video Bridge (T2V-B) and Video-to-Text Bridge (V2T-B). They allow LLMs to guide the generation of visually-grounded text plans and textual-grounded video plans. To address the scarcity of datasets suitable for MPP, we have curated a new dataset called Daily-Life Task Procedural Plans (Daily-PP). We conduct comprehensive experiments and benchmarks to evaluate human preferences (regarding textual and visual informativeness, temporal coherence, and plan accuracy). Our VG-TVP method outperforms unimodal baselines on the Daily-PP dataset.</p>
<p>Please check out our paper "VG-TVP: Multimodal Procedural Planning via Visually Grounded Text-Video Prompting"!</p>

<p>When you reach the dataset folder, you will see these subfiles:</p>
<ol>
  <li>All_Given_Videos_for_Dataset: Consists of all tasks' videos.</li>
  <li>Dataset: Consists of all tasks' details.</li>
  <li>Evaluations_and_Results: Consists of Automatic_Evaluations_All_Final_Results, LLM_Evaluations_Results, Video_CLIP_Scores_Randomly_Selected and Word_Counter_Results.</li> 
  <li>Figures_All: This file shows all figures used in the paper.</li>
  <li>Final_Paper: It consists of the final paper.</li>
  <li>Generated_Videos: It consists of all Seen and Unseen tasks' videos.</li>
  <li>Supplementary_Materials: This folder consists of supplementary materials that were submitted to the conference.</li>
  <li>Well_prepared_T2V_Generation_from_VG-TVP: Consists of well-prepared generated texts and relevant videos.</li>


<h2>Overview</h2>

<h3>1. VG-TVP </h3>
<p>In this paper, we aim to improve PP by generating multimodal content from different resources, including IVs. We are interested in enhancing human understanding by integrating visually grounded text and action-based video generations. We cast the problem as MPP via Visually Grounded Text-Video Prompting (VG-TVP). VG-TVP generates video-enhanced action and state procedures given text descriptions of a task and IVs, which is in contrast to generating image plans and using their descriptions as text plans.</p>

<img width="1158" alt="Model_idea1" src="https://github.com/user-attachments/assets/ab30a191-5021-4e25-bafb-f31349907ca1">
<p>Figure 1: VG-TVP Model: Given the textual input and multiple instructional videos, VG-TVP generates visually grounded textual plans and video plans by using V2T-B and T2V-B. ChatGPT 3.5 is used to reorganize all captions to generate FoC.</p>

<p>We propose two key ideas in a methodology. The first idea involves generating MPPs for the tasks by exploiting their IVs, which is called "SEEN" (Idea 1). For the "SEEN" tasks, the model utilizes the relevant task IVs and captures their captions. Finally, video captions of these "SEEN" tasks and vanilla text plans are aligned to generate visually grounded text and video plans. The "UNSEEN" tasks (Idea 2) which the model has not previously encountered, involve generating MPPs for tasks that lack IVs. For example, "Cooking Kimchi Fried Rice" and "Cooking Szechuan Chicken" are two "SEEN" tasks under the Daily-PP dataset. We propose an exploration of "Cooking Chicken Fried Rice" as an "UNSEEN" task by utilizing the captions of "Cooking Kimchi Fried Rice" and "Cooking Szechuan Chicken".</p>


<h4>2. Problem Statement</h4>
<p>We formulate the problem as a visually grounded textual-video pairs alignment and generation task. $G^V$ is given multiple IVs which represent high-level goal videos, and $G^T$ is given textual input which denotes high-level goal texts, provided by the user in natural language. The model's output is a comprehensive multimodal procedural plan, represented as Goal Plan, $G^P$. It comprises the final sequence of visually-grounded text plan $TP = \{\{pt_1, pc_1\}, \{pt_2, pc_2\}, ... , \{pt_n, pc_n\}\}$  and video plan $VP = \{v_1, v_2, ... , v_n\}$ pairs. In this notation, $pt_i$ represents the text of the final revised textual plan and $pc_i$ denotes its corresponding context. Consequently, $G^P = \{TP, VP\}$ is generated by merging $3$ multimodal features: vanilla text generation, video captioning via V2T-Bridge, and video generation via T2V-Bridge.</p>

<h4>3. Fusion of Captioning</h4>
<p>We employ the VLog model which integrates the capabilities of ChatGPT, BLIP2, GRIT, and Whisper models. VLog generates video captions from $3$ modalities: image, region, and audio. Our framework excludes audio captions as some videos lacked speech or significant auditory content. Thus, we follow the visual information. V2T-B is the method that textualizes the scenes of IVs by using a video captioning algorithm. For example, in the "apple juice" task, given that the $3rd$ caption of the $1st$ IV represents the step "wooden cutting board with sliced apples on it" between $17-26$ seconds (sec), similar steps appear in the $4th$ IV at $39-50$ sec, and the $7th$ IV at $9-19$ sec in the $2nd$ step (not in $3rd$). In other IVs' captions, steps similar to "pouring water into a blender" are included in different steps. Therefore, FoC reorders and aligns the steps for vanilla textual matching (Figure 15 in the Appendix). FoC is insufficient for completing the MPP alone. It provides additional visual information to VG-TVP for generating MPP tasks. Consequently, we formulate the V2T-B as $FoC$ = $f_{captions}$ $\oplus$ ($G^V$), $f_{prompt}(description)$. The "Description" prompt is "According to the captions above, then prepare the logical video captions for the task procedures of the <[TASK]> step-by-step in a sentence template of (person, verb, and action)?". Detailed video captions and FoC can be found in the supplementary materials.</p>

<img width="300" alt="FOC" src="https://github.com/user-attachments/assets/d1ceb7af-b3d0-4139-b2e5-79c99b2782c1">
<p>Figure 2: FoC: FoC captures and fuses IVs' captions. Then, it injects into the system by aligning them with vanilla textual.</p>

<img width="1158" alt="foc_align" src="https://github.com/user-attachments/assets/0ca900cd-1fba-4ce7-9698-32bc454bb83e">
<p>Figure 3: The fundamental representation of the FoC to show how different mismatched steps in IVs are aligned and prepared for generating an accurate MPP content.</p>


<h4>4. Daily-PP Dataset</h4>
<p>The Daily-PP (Daily-Life Task Procedural Plans) consists of 5 domains (Breakfast, Dinner, Drink, Hobby&Crafts, and Home&Garage), 50 seen tasks, and 15 unseen tasks. Seen tasks include 7 or 10 IVs from YouTube, depending on the video density for each task. Moreover, $3$ domains (Breakfast, Drink, and Dinner) have unseen tasks such as egg benedict, carrot mango lassi, and chicken fried rice. Unseen tasks are those without any IVs. The model generates their MPPs by using their vanilla text plan with $2$ relevant seen tasks' video captions. For example, VG-TVP uses the video captions of "carrot juice" & "mango lassi" tasks to generate the MPP of "carrot mango lassi".</p>

<img width="1158" alt="daily-pp" src="https://github.com/user-attachments/assets/a351c7c8-582d-4a1d-a27a-7eae8e8405a7">
<p>Figure 4: Daily-PP: Daily Life Task Procedures Dataset Structure.</p>

<h4>5. Experiments</h4>
<p>VG-TVP exploits vanilla text and FoC to generate visually grounded MPP content to assist individuals. We use a Win-Tie-Lose comparison on 50 seen and 15 unseen tasks, involving 28 human subjects for benchmarking. VG-TVP generated 2,504 videos for "seen" and 687 for "unseen" tasks, while baselines produced 2,701 and 681 vanilla textual videos, respectively. We included $2$ tasks from WikiPlan\&RecipePlan (Lu et al. 2023b), to facilitate a fair benchmarking. Moreover, we conduct one more comparison with $2$ different prompts (in the Appendix). This comparison aims to compare the effectiveness of injecting human orientation into text and video plans with VG-TVP against prompting. The qualitative results display that human orientation is more effectively integrated with VG-TVP. Additionally, we measure the textual relevance between generated baselines' and VG-TVP's text plans with reference text plans using BLEU (Papineni et al. 2002), and METEOR (Banerjee and Lavie 2005). Finally, we design an LLM (via ChatGPT4o) (with Socratic Method (Chang 2023)) evaluation protocol to evaluate baselines and VG-TVP on $4$ aspects as in the human evaluation metric. Each scored out of $25$ points, to assess the quality of task plans. The details are in the Appendix.</p>

<p>Existing metrics such as BLEU and METEOR evaluate text similarity by comparing generated texts with reference texts. However, they have limitations in MPP tasks. In cases where tasks do not have strict laws or steps, it cannot be assumed there is a single ground truth (GT). Daily life tasks such as "Hanging a Mirror", and "Cooking Pancakes" lack definitive instructions and can vary widely. They cannot be deemed as having only one correct GT. Thus, a human evaluation survey is the optimal metric for assessing tasks aimed at generating MPPs. An example of a survey is in the Appendix.</p>

<h4>5.1 Results</h4>
<p>We employ 7 different LLMs to generate vanilla text plans which are (1) LLama2-7B-q4, (2) LLama2-7B-q8, (3) LLama2-13B-q4, (4) LLama2-13B-q8, (5) Mistral-7B-q4, (6) Mistral-7B-q8 and (7) ChatGPT3.5.</p>

<h4>I. Human Evaluation Metric</h4>
<p> Existing metrics such as BLEU and METEOR evaluate text similarity by comparing generated texts with reference texts. However, they have limitations in MPP tasks. In cases where tasks do not have strict laws or steps, it cannot be assumed there is a single ground truth (GT). Daily life tasks such as "Hanging a Mirror", and "Cooking Pancakes" lack definitive instructions and can vary widely. They cannot be deemed as having only one correct GT. Thus, a human evaluation survey is the optimal metric for assessing tasks aimed at generating MPPs.</p>


<p> Table 1: Percentages (%) of human evaluation comparisons between VG-TVP (Ours), baseline models by employing different LLMs (first 7 rows), and TIP (Lu et al. 2023b) for SEEN tasks. Win represents the preferred option for VG-TVP.</p>

<table class="tg"><thead>
  <tr>
    <th class="tg-c3ow">VG-TVP (Ours)</th>
    <th class="tg-c3ow" colspan="3">Textual Informative </th>
    <th class="tg-c3ow" colspan="3">Visual Informative </th>
    <th class="tg-c3ow" colspan="3">Temporal Coherence </th>
    <th class="tg-c3ow" colspan="3">Plan Accuracy </th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-c3ow">versus</td>
    <td class="tg-c3ow">Win</td>
    <td class="tg-c3ow">Tie</td>
    <td class="tg-c3ow">Lose</td>
    <td class="tg-c3ow">Win</td>
    <td class="tg-c3ow">Tie</td>
    <td class="tg-c3ow">Lose</td>
    <td class="tg-c3ow">Win</td>
    <td class="tg-c3ow">Tie</td>
    <td class="tg-c3ow">Lose</td>
    <td class="tg-c3ow">Win</td>
    <td class="tg-c3ow">Tie</td>
    <td class="tg-c3ow">Lose</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Llama2-7B-q4</td>
    <td class="tg-c3ow">44.00 </td>
    <td class="tg-c3ow">40.00</td>
    <td class="tg-c3ow">16.00</td>
    <td class="tg-c3ow">74.00</td>
    <td class="tg-c3ow">2.00</td>
    <td class="tg-c3ow"> 24.00</td>
    <td class="tg-c3ow">68.00</td>
    <td class="tg-c3ow">8.00</td>
    <td class="tg-c3ow">24.00</td>
    <td class="tg-c3ow">74.00</td>
    <td class="tg-c3ow">6.00</td>
    <td class="tg-c3ow">20.00</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Llama2-7B-q8</td>
    <td class="tg-c3ow">52.00</td>
    <td class="tg-c3ow">22.00</td>
    <td class="tg-c3ow">26.00</td>
    <td class="tg-c3ow">62.00</td>
    <td class="tg-c3ow">18.00</td>
    <td class="tg-c3ow">20.00</td>
    <td class="tg-c3ow">62.00</td>
    <td class="tg-c3ow">24.00</td>
    <td class="tg-c3ow">14.00</td>
    <td class="tg-c3ow">70.00</td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">12.00</span></td>
    <td class="tg-c3ow">18.00</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Llama2-13B-q4</td>
    <td class="tg-c3ow">40.00</td>
    <td class="tg-c3ow">24.00</td>
    <td class="tg-c3ow">36.00</td>
    <td class="tg-c3ow">46.00</td>
    <td class="tg-c3ow">10.00</td>
    <td class="tg-c3ow">44.00</td>
    <td class="tg-c3ow">42.00</td>
    <td class="tg-c3ow">34.00</td>
    <td class="tg-c3ow">24.00</td>
    <td class="tg-c3ow">40.00</td>
    <td class="tg-c3ow">18.00</td>
    <td class="tg-c3ow">42.00</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Llama2-13B-q8</td>
    <td class="tg-c3ow">20.00</td>
    <td class="tg-c3ow">48.00</td>
    <td class="tg-c3ow">32.00</td>
    <td class="tg-c3ow">56.00</td>
    <td class="tg-c3ow">18.00</td>
    <td class="tg-c3ow">26.00</td>
    <td class="tg-c3ow">36.00</td>
    <td class="tg-c3ow">44.00</td>
    <td class="tg-c3ow">20.00</td>
    <td class="tg-c3ow">46.00</td>
    <td class="tg-c3ow">22.00</td>
    <td class="tg-c3ow">32.00</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Mistral-7B-q4</td>
    <td class="tg-c3ow">58.00</td>
    <td class="tg-c3ow">26.00</td>
    <td class="tg-c3ow">16.00</td>
    <td class="tg-c3ow">56.00</td>
    <td class="tg-c3ow">12.00</td>
    <td class="tg-c3ow">32.00</td>
    <td class="tg-c3ow">52.00</td>
    <td class="tg-c3ow">20.00</td>
    <td class="tg-c3ow">28.00</td>
    <td class="tg-c3ow">48.00</td>
    <td class="tg-c3ow">28.00</td>
    <td class="tg-c3ow">24.00</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Mistral-7B-q8</td>
    <td class="tg-c3ow">54.00</td>
    <td class="tg-c3ow">22.00</td>
    <td class="tg-c3ow">24.00</td>
    <td class="tg-c3ow">58.00</td>
    <td class="tg-c3ow">14.00</td>
    <td class="tg-c3ow">28.00</td>
    <td class="tg-c3ow">46.00</td>
    <td class="tg-c3ow">34.00</td>
    <td class="tg-c3ow">20.00</td>
    <td class="tg-c3ow">56.00</td>
    <td class="tg-c3ow">16.00</td>
    <td class="tg-c3ow">28.00</td>
  </tr>
  <tr>
    <td class="tg-c3ow">ChatGPT3.5.</td>
    <td class="tg-c3ow">40.00</td>
    <td class="tg-c3ow">24.00</td>
    <td class="tg-c3ow">36.00</td>
    <td class="tg-c3ow">56.00</td>
    <td class="tg-c3ow">12.00</td>
    <td class="tg-c3ow">32.00</td>
    <td class="tg-c3ow">54.00</td>
    <td class="tg-c3ow">14.00</td>
    <td class="tg-c3ow">32.00</td>
    <td class="tg-c3ow">56.00</td>
    <td class="tg-c3ow">10.00</td>
    <td class="tg-c3ow">34.00</td>
  </tr>
  <tr>
    <td class="tg-c3ow">TIP Model</td>
    <td class="tg-c3ow">71.43</td>
    <td class="tg-c3ow">21.43</td>
    <td class="tg-c3ow">7.14</td>
    <td class="tg-c3ow">57.14</td>
    <td class="tg-c3ow">21.43</td>
    <td class="tg-c3ow">21.43</td>
    <td class="tg-c3ow">42.86</td>
    <td class="tg-c3ow">35.71</td>
    <td class="tg-c3ow">21.43</td>
    <td class="tg-c3ow">42.86</td>
    <td class="tg-c3ow">28.57</td>
    <td class="tg-c3ow">28.57</td>
  </tr>
</tbody></table>


<p> Table 2: Percentages (%) of human evaluation comparisons between VG-TVP (Ours) and baseline models by employing different LLMs (q: quantization) for UNSEEN tasks. WIN, and LOSE represent the better and worse results of VG-TVP, respectively.</p>
<table class="tg"><thead>
  <tr>
    <th class="tg-c3ow">VG-TVP (Ours)</th>
    <th class="tg-c3ow" colspan="3">Textual Informative </th>
    <th class="tg-c3ow" colspan="3">Visual Informative </th>
    <th class="tg-c3ow" colspan="3">Temporal Coherence </th>
    <th class="tg-c3ow" colspan="3">Plan Accuracy </th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-c3ow">versus</td>
    <td class="tg-c3ow">Win</td>
    <td class="tg-c3ow">Tie</td>
    <td class="tg-c3ow">Lose</td>
    <td class="tg-c3ow">Win</td>
    <td class="tg-c3ow">Tie</td>
    <td class="tg-c3ow">Lose</td>
    <td class="tg-c3ow">Win</td>
    <td class="tg-c3ow">Tie</td>
    <td class="tg-c3ow">Lose</td>
    <td class="tg-c3ow">Win</td>
    <td class="tg-c3ow">Tie</td>
    <td class="tg-c3ow">Lose</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Llama2-7B-q4</td>
    <td class="tg-c3ow">40.00 </td>
    <td class="tg-c3ow">26.67</td>
    <td class="tg-c3ow">33.33</td>
    <td class="tg-c3ow">60.00</td>
    <td class="tg-c3ow">26.67</td>
    <td class="tg-c3ow">13.33</td>
    <td class="tg-c3ow">60.00</td>
    <td class="tg-c3ow">26.67</td>
    <td class="tg-c3ow">13.33</td>
    <td class="tg-c3ow">66.67</td>
    <td class="tg-c3ow">20.00</td>
    <td class="tg-c3ow">13.33</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Llama2-7B-q8</td>
    <td class="tg-c3ow">66.67</td>
    <td class="tg-c3ow">6.67</td>
    <td class="tg-c3ow">26.67</td>
    <td class="tg-c3ow">53.33</td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">26.67</span></td>
    <td class="tg-c3ow">20.00</td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">53.33</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">26.67</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">20.00</span></td>
    <td class="tg-c3ow">60.00</td>
    <td class="tg-c3ow">6.67</td>
    <td class="tg-c3ow">33.33</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Llama2-13B-q4</td>
    <td class="tg-c3ow">26.67</td>
    <td class="tg-c3ow">53.33</td>
    <td class="tg-c3ow">20.00</td>
    <td class="tg-c3ow">46.67</td>
    <td class="tg-c3ow">13.33</td>
    <td class="tg-c3ow">40.00</td>
    <td class="tg-c3ow">53.33</td>
    <td class="tg-c3ow">26.67</td>
    <td class="tg-c3ow">20.00</td>
    <td class="tg-c3ow">40.00</td>
    <td class="tg-c3ow">33.33</td>
    <td class="tg-c3ow">26.67</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Llama2-13B-q8</td>
    <td class="tg-c3ow">53.33</td>
    <td class="tg-c3ow">40.00</td>
    <td class="tg-c3ow">6.67</td>
    <td class="tg-c3ow">60.00</td>
    <td class="tg-c3ow">33.33</td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">6.67</span></td>
    <td class="tg-c3ow">80.00</td>
    <td class="tg-c3ow">0.00</td>
    <td class="tg-c3ow">20.00</td>
    <td class="tg-c3ow">66.67</td>
    <td class="tg-c3ow">13.33</td>
    <td class="tg-c3ow">20.00</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Mistral-7B-q4</td>
    <td class="tg-c3ow">40.00</td>
    <td class="tg-c3ow">40.00</td>
    <td class="tg-c3ow">20.00</td>
    <td class="tg-c3ow">53.33</td>
    <td class="tg-c3ow">26.67</td>
    <td class="tg-c3ow">20.00</td>
    <td class="tg-c3ow">46.67</td>
    <td class="tg-c3ow">46.67</td>
    <td class="tg-c3ow">6.67</td>
    <td class="tg-c3ow">40.00</td>
    <td class="tg-c3ow">53.33</td>
    <td class="tg-c3ow">6.67</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Mistral-7B-q8</td>
    <td class="tg-c3ow">73.33</td>
    <td class="tg-c3ow">20.00</td>
    <td class="tg-c3ow">6.67</td>
    <td class="tg-c3ow">86.67</td>
    <td class="tg-c3ow">0.00</td>
    <td class="tg-c3ow">13.33</td>
    <td class="tg-c3ow">66.67</td>
    <td class="tg-c3ow">26.67</td>
    <td class="tg-c3ow">6.67</td>
    <td class="tg-c3ow">73.33</td>
    <td class="tg-c3ow">20.00</td>
    <td class="tg-c3ow">6.67</td>
  </tr>
  <tr>
    <td class="tg-c3ow">ChatGPT3.5.</td>
    <td class="tg-c3ow">13.33</td>
    <td class="tg-c3ow">80.00</td>
    <td class="tg-c3ow">6.67</td>
    <td class="tg-c3ow">60.00</td>
    <td class="tg-c3ow">6.67</td>
    <td class="tg-c3ow">33.33</td>
    <td class="tg-c3ow">20.00</td>
    <td class="tg-c3ow">40.00</td>
    <td class="tg-c3ow">40.00</td>
    <td class="tg-c3ow">26.67</td>
    <td class="tg-c3ow">33.33</td>
    <td class="tg-c3ow">40.00</td>
  </tr>
</tbody></table>

<h5>II. Quantitative Analysis</h5>

<p> Table 3: Automatic evaluations on 50 seen tasks from Daily-PP. Generated Baselines’ (Base.) and VG-TVP’s text plans are compared with the reference textual.</p>
<table class="tg"><thead>
  <tr>
    <th class="tg-c3ow">Models</th>
    <th class="tg-c3ow" colspan="2">BLEU</th>
    <th class="tg-c3ow" colspan="2">METEOR</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow">Baseline</td>
    <td class="tg-c3ow">VG-TVP</td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">Baseline</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">VG-TVP</span></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Llama2-7B-q4</td>
    <td class="tg-c3ow">0.013</td>
    <td class="tg-c3ow">0.013</td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.089</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.082</span></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Llama2-7B-q8</td>
    <td class="tg-c3ow">0.014</td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.011</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.082</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.075</span></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Llama2-13B-q4</td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.012</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.011</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.067</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.071</span></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Llama2-13B-q8</td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.014</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.012</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.082</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.073</span></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Mistral-7B-q4</td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.012</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.014</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.091</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.093</span></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Mistral-7B-q8</td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.013</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.015</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.084</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.094</span></td>
  </tr>
  <tr>
    <td class="tg-c3ow">ChatGPT3.5.</td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.017</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.024</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.072</span></td>
    <td class="tg-c3ow"><span style="font-weight:400;font-style:normal">0.085</span></td>
  </tr>
</tbody></table>


<p>We design another experiment with CLIP scores in different frame rates (5,10, 15, and 20) to highlight the impact of generating videos instead of single static images. That experiment’s results show the average of mean similarity scores (MSS) for seen and unseen tasks of baselines, VGTVP; and also benchmark comparisons. Results display that VG-TVP consistently outperforms baselines and TIP, highlighting the effectiveness of our approach across various task types and frame rates.</p>
<p> Table 4:  Average MSS of Baselines and VG-TVP for Seen Tasks (Idea 1).</p>
<table class="tg"><thead>
  <tr>
    <th class="tg-c3ow">Frame Rate</th>
    <th class="tg-c3ow" colspan="2"><span style="font-weight:400;font-style:normal">Seen Tasks (Idea 1)</span></th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-7btt"></td>
    <td class="tg-c3ow">Baselines</td>
    <td class="tg-c3ow">VG-TVP</td>
  </tr>
  <tr>
    <td class="tg-c3ow">20</td>
    <td class="tg-c3ow">0.294634078</td>
    <td class="tg-c3ow">0.318871641</td>
  </tr>
  <tr>
    <td class="tg-c3ow">15</td>
    <td class="tg-c3ow">0.296215234</td>
    <td class="tg-c3ow">0.320155963</td>
  </tr>
  <tr>
    <td class="tg-baqh">10</td>
    <td class="tg-baqh">0.296341701</td>
    <td class="tg-baqh">0.320829739</td>
  </tr>
  <tr>
    <td class="tg-baqh">5</td>
    <td class="tg-baqh">0.296913945</td>
    <td class="tg-baqh">0.321579028</td>
  </tr>
</tbody>
</table>

<p> Table 5:  Average MSS of Baselines and VG-TVP for Unseen Tasks (Idea 2).</p>
<table class="tg"><thead>
  <tr>
    <th class="tg-c3ow">Frame Rate</th>
    <th class="tg-c3ow" colspan="2"><span style="font-weight:400;font-style:normal">Seen Tasks (Idea 1)</span></th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-7btt"></td>
    <td class="tg-c3ow">Baselines</td>
    <td class="tg-c3ow">VG-TVP</td>
  </tr>
  <tr>
    <td class="tg-c3ow">20</td>
    <td class="tg-c3ow">0.3009426</td>
    <td class="tg-c3ow">0.3268314</td>
  </tr>
  <tr>
    <td class="tg-c3ow">15</td>
    <td class="tg-c3ow">0.3020536</td>
    <td class="tg-c3ow">0.3290246</td>
  </tr>
  <tr>
    <td class="tg-baqh">10</td>
    <td class="tg-baqh">0.3025034</td>
    <td class="tg-baqh">0.3290118</td>
  </tr>
  <tr>
    <td class="tg-baqh">5</td>
    <td class="tg-baqh">0.3027169</td>
    <td class="tg-baqh">0.3301511</td>
  </tr>
</tbody>
</table>


<p>LLM Evaluation Protocol with Socratic Method: The Socratic Model represents the resolution of complex tasks through a series of questions. It involves various principles (Chang 2023), such as identifying key points of the tasks, proposing cause-and-effect relationships for tasks, presenting text plan examples, and asking evaluation aspects questions. Thus, we leverage each of these principles using ChatGPT-4o. The evaluation protocol evaluates baselines and VG-TVP on 4 aspects as in the human evaluation metric. There are 5 and 3 randomly chosen tasks from each domain that were evaluated for seen (Candy Bouquet, Change a Tire, Kimchi Fried Rice, Moka Pot Coffee, and Pancake) and unseen (Chicken Fried Rice, Carrot Mango Lassi and Egg Benedict) tasks, respectively. This evaluation considers the semantic relevance of the task plans generated by baselines and VG-TVP. Each scored out of 25 points, thereby out of 100 points in total. All results for the seen and unseen tasks are shown in Table 6 and Table 7 in terms of Textual Informativeness, Visual Informativeness, Temporal Alignment, and Plan Accuracy.</p>
<p> Table 6: Average results of LLM evaluations (chosen from 5 random tasks, 1 from each domain) for SEEN tasks. ChatGPT-4o compares baselines and VG-TVP textual.</p>
<table class="tg"><thead>
  <tr>
    <th class="tg-c3ow">Models</th>
    <th class="tg-c3ow"><span style="font-weight:400;font-style:normal">Textual Informative</span></th>
    <th class="tg-c3ow"><span style="font-weight:400;font-style:normal">Visual Informative</span></th>
    <th class="tg-c3ow">Temporal Coherence</th>
    <th class="tg-c3ow">Plan Accuracy</th>
    <th class="tg-baqh">Total</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-7btt"><span style="font-weight:700;font-style:normal">Llama2-7B-q4</span></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-baqh"></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Baseline</td>
    <td class="tg-c3ow">20.6</td>
    <td class="tg-c3ow">7.5</td>
    <td class="tg-c3ow">18</td>
    <td class="tg-c3ow">18.9</td>
    <td class="tg-baqh">65</td>
  </tr>
  <tr>
    <td class="tg-c3ow">VG-TVP</td>
    <td class="tg-c3ow">23.6</td>
    <td class="tg-c3ow">22.7</td>
    <td class="tg-c3ow">22</td>
    <td class="tg-c3ow">23</td>
    <td class="tg-baqh">91.3</td>
  </tr>
  <tr>
    <td class="tg-baqh"><span style="font-weight:700;font-style:normal">Llama2-7B-q8</span></td>
    <td class="tg-baqh"></td>
    <td class="tg-baqh"></td>
    <td class="tg-baqh"></td>
    <td class="tg-baqh"></td>
    <td class="tg-baqh"></td>
  </tr>
  <tr>
    <td class="tg-baqh">Baseline</td>
    <td class="tg-baqh">20.8</td>
    <td class="tg-baqh">7.4</td>
    <td class="tg-baqh">18.4</td>
    <td class="tg-baqh">19</td>
    <td class="tg-baqh">65.6</td>
  </tr>
  <tr>
    <td class="tg-baqh">VG-TVP</td>
    <td class="tg-baqh">23.7</td>
    <td class="tg-baqh">22.2</td>
    <td class="tg-baqh">21.8</td>
    <td class="tg-baqh"><span style="font-weight:400;font-style:normal">22.8</span></td>
    <td class="tg-baqh">90.5</td>
  </tr>
  <tr>
    <td class="tg-c3ow"><span style="font-weight:bold">Llama2-13B-q4</span></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-baqh"></td>
  </tr>
  <tr>
    <td class="tg-baqh">Baseline</td>
    <td class="tg-baqh">21.2</td>
    <td class="tg-baqh">8.3</td>
    <td class="tg-baqh">18.8</td>
    <td class="tg-baqh">19.5</td>
    <td class="tg-baqh">67.8</td>
  </tr>
  <tr>
    <td class="tg-baqh">VG-TVP</td>
    <td class="tg-baqh">24.1</td>
    <td class="tg-baqh">23.2</td>
    <td class="tg-baqh">22</td>
    <td class="tg-baqh">23.2</td>
    <td class="tg-baqh">92.5</td>
  </tr>
  <tr>
    <td class="tg-7btt">Llama2-13B-q8</td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-baqh"></td>
  </tr>
  <tr>
    <td class="tg-baqh">Baseline</td>
    <td class="tg-baqh">21.3</td>
    <td class="tg-baqh">8.1</td>
    <td class="tg-baqh">18.7</td>
    <td class="tg-baqh">19.3</td>
    <td class="tg-baqh">67.4</td>
  </tr>
  <tr>
    <td class="tg-baqh">VG-TVP</td>
    <td class="tg-baqh">24</td>
    <td class="tg-baqh">22.6</td>
    <td class="tg-baqh">22.2</td>
    <td class="tg-baqh">23.5</td>
    <td class="tg-baqh">92.3</td>
  </tr>
  <tr>
    <td class="tg-7btt">Mistral-7B-q4</td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-baqh"></td>
  </tr>
  <tr>
    <td class="tg-baqh">Baseline</td>
    <td class="tg-baqh">20.4</td>
    <td class="tg-baqh">7.6</td>
    <td class="tg-baqh">18.2</td>
    <td class="tg-baqh">19.3</td>
    <td class="tg-baqh">65.5</td>
  </tr>
  <tr>
    <td class="tg-baqh">VG-TVP</td>
    <td class="tg-baqh">23.7</td>
    <td class="tg-baqh">22.6</td>
    <td class="tg-baqh">21.9</td>
    <td class="tg-baqh">22.9</td>
    <td class="tg-baqh">91.1</td>
  </tr>
  <tr>
    <td class="tg-7btt">Mistral-7B-q8</td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-baqh"></td>
  </tr>
  <tr>
    <td class="tg-baqh">Baseline</td>
    <td class="tg-baqh">20.9</td>
    <td class="tg-baqh">7.9</td>
    <td class="tg-baqh">18.3</td>
    <td class="tg-baqh">19.7</td>
    <td class="tg-baqh">66.8</td>
  </tr>
  <tr>
    <td class="tg-baqh">VG-TVP</td>
    <td class="tg-baqh">23.8</td>
    <td class="tg-baqh">22.6</td>
    <td class="tg-baqh">22.2</td>
    <td class="tg-baqh">23</td>
    <td class="tg-baqh">91.6</td>
  </tr>
  <tr>
    <td class="tg-7btt">ChatGPT3.5.</td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-baqh"></td>
  </tr>
  <tr>
    <td class="tg-baqh">Baseline</td>
    <td class="tg-baqh">22.2</td>
    <td class="tg-baqh">8.6</td>
    <td class="tg-baqh">19</td>
    <td class="tg-baqh">19.8</td>
    <td class="tg-baqh">69.6</td>
  </tr>
  <tr>
    <td class="tg-baqh">VG-TVP</td>
    <td class="tg-baqh">24</td>
    <td class="tg-baqh">22.6</td>
    <td class="tg-baqh">21.9</td>
    <td class="tg-baqh">23.6</td>
    <td class="tg-baqh">92.1</td>
  </tr>
</tbody></table>

<p> Table 7: Average results of LLM evaluations (chosen from 3 random tasks, 1 from each domain) for UNSEEN tasks. ChatGPT-4o compares baselines and VG-TVP textual.</p>
<table class="tg"><thead>
  <tr>
    <th class="tg-c3ow">Models</th>
    <th class="tg-c3ow"><span style="font-weight:400;font-style:normal">Textual Informative</span></th>
    <th class="tg-c3ow"><span style="font-weight:400;font-style:normal">Visual Informative</span></th>
    <th class="tg-c3ow">Temporal Coherence</th>
    <th class="tg-c3ow">Plan Accuracy</th>
    <th class="tg-baqh">Total</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-7btt"><span style="font-weight:700;font-style:normal">Llama2-7B-q4</span></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-baqh"></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Baseline</td>
    <td class="tg-c3ow">21.3</td>
    <td class="tg-c3ow">9.8</td>
    <td class="tg-c3ow">18.7</td>
    <td class="tg-c3ow">19.3</td>
    <td class="tg-baqh">69.2</td>
  </tr>
  <tr>
    <td class="tg-c3ow">VG-TVP</td>
    <td class="tg-c3ow">23.0</td>
    <td class="tg-c3ow">22.0</td>
    <td class="tg-c3ow">21.7</td>
    <td class="tg-c3ow">23.0</td>
    <td class="tg-baqh">89.7</td>
  </tr>
  <tr>
    <td class="tg-baqh"><span style="font-weight:700;font-style:normal">Llama2-7B-q8</span></td>
    <td class="tg-baqh"></td>
    <td class="tg-baqh"></td>
    <td class="tg-baqh"></td>
    <td class="tg-baqh"></td>
    <td class="tg-baqh"></td>
  </tr>
  <tr>
    <td class="tg-baqh">Baseline</td>
    <td class="tg-baqh">21.0</td>
    <td class="tg-baqh">10.2</td>
    <td class="tg-baqh">18.3</td>
    <td class="tg-baqh">19.3</td>
    <td class="tg-baqh">68.8</td>
  </tr>
  <tr>
    <td class="tg-baqh">VG-TVP</td>
    <td class="tg-baqh">23.2</td>
    <td class="tg-baqh">22.5</td>
    <td class="tg-baqh">22.0</td>
    <td class="tg-baqh">22.7</td>
    <td class="tg-baqh">90.3</td>
  </tr>
  <tr>
    <td class="tg-c3ow"><span style="font-weight:bold">Llama2-13B-q4</span></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-baqh"></td>
  </tr>
  <tr>
    <td class="tg-baqh">Baseline</td>
    <td class="tg-baqh">19.2</td>
    <td class="tg-baqh">11.0</td>
    <td class="tg-baqh">19.2</td>
    <td class="tg-baqh">19.8</td>
    <td class="tg-baqh">71.7</td>
  </tr>
  <tr>
    <td class="tg-baqh">VG-TVP</td>
    <td class="tg-baqh">23.2</td>
    <td class="tg-baqh">22.5</td>
    <td class="tg-baqh">22.0</td>
    <td class="tg-baqh">23.0</td>
    <td class="tg-baqh">90.7</td>
  </tr>
  <tr>
    <td class="tg-7btt">Llama2-13B-q8</td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-baqh"></td>
  </tr>
  <tr>
    <td class="tg-baqh">Baseline</td>
    <td class="tg-baqh">21.5</td>
    <td class="tg-baqh">10.7</td>
    <td class="tg-baqh">18.8</td>
    <td class="tg-baqh">20.3</td>
    <td class="tg-baqh">71.3</td>
  </tr>
  <tr>
    <td class="tg-baqh">VG-TVP</td>
    <td class="tg-baqh">23.5</td>
    <td class="tg-baqh">22.5</td>
    <td class="tg-baqh">22.0</td>
    <td class="tg-baqh">23.7</td>
    <td class="tg-baqh">91.7</td>
  </tr>
  <tr>
    <td class="tg-7btt">Mistral-7B-q4</td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-baqh"></td>
  </tr>
  <tr>
    <td class="tg-baqh">Baseline</td>
    <td class="tg-baqh">21.5</td>
    <td class="tg-baqh">12.2</td>
    <td class="tg-baqh">18.7</td>
    <td class="tg-baqh">20.0</td>
    <td class="tg-baqh">72.3</td>
  </tr>
  <tr>
    <td class="tg-baqh">VG-TVP</td>
    <td class="tg-baqh">23.8</td>
    <td class="tg-baqh">23.0</td>
    <td class="tg-baqh">22.3</td>
    <td class="tg-baqh">23.2</td>
    <td class="tg-baqh">92.3</td>
  </tr>
  <tr>
    <td class="tg-7btt">Mistral-7B-q8</td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-baqh"></td>
  </tr>
  <tr>
    <td class="tg-baqh">Baseline</td>
    <td class="tg-baqh">22.3</td>
    <td class="tg-baqh">13.0</td>
    <td class="tg-baqh">19.5</td>
    <td class="tg-baqh">20.5</td>
    <td class="tg-baqh">75.3</td>
  </tr>
  <tr>
    <td class="tg-baqh">VG-TVP</td>
    <td class="tg-baqh">24.0</td>
    <td class="tg-baqh">23.0</td>
    <td class="tg-baqh">22.3</td>
    <td class="tg-baqh">23.3</td>
    <td class="tg-baqh">92.7</td>
  </tr>
  <tr>
    <td class="tg-7btt">ChatGPT3.5.</td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-c3ow"></td>
    <td class="tg-baqh"></td>
  </tr>
  <tr>
    <td class="tg-baqh">Baseline</td>
    <td class="tg-baqh">22.5</td>
    <td class="tg-baqh">11.0</td>
    <td class="tg-baqh">19.8</td>
    <td class="tg-baqh">20.8</td>
    <td class="tg-baqh">74.2</td>
  </tr>
  <tr>
    <td class="tg-baqh">VG-TVP</td>
    <td class="tg-baqh">24.0</td>
    <td class="tg-baqh">22.8</td>
    <td class="tg-baqh">22.5</td>
    <td class="tg-baqh">23.7</td>
    <td class="tg-baqh">93.0</td>
  </tr>
</tbody></table>



<h4>III. Qualitative Results</h4>
<p>The generated vanilla text plans successfully verbalize procedural information across many tasks. However, using vanilla text plans may not achieve the same efficiency for visuals while generating videos. We hypothesize that augmenting these vanilla textual with IVs can enhance MPP, thereby more effectively assisting individuals. Our results and comparative analyses with VG-TVP and baselines confirm this hypothesis. We generate baseline models' videos using the text-context structure of vanilla textual.</p>

<img width="1158" alt="qual_1" src="https://github.com/user-attachments/assets/0e65ffa6-9dba-4b7a-809b-98fdb738ce86">
<p>Figure 5: Qualitative comparison between Llama2-13B-q8 Model and VG-T2V (Ours). Visuals (orange) are used to generate video plans. VG-TVP can increase the number of steps to generate the MPP more informative and accurate.</p>

<img width="1158" alt="qual_2" src="https://github.com/user-attachments/assets/f56b7113-1586-45e8-9c62-22e2266ee206">
<p>Figure 6: Qualitative Comparison Example with Visualized Instruction Prompting by Llama2-7B-q4, (Task: How to make pancakes?).</p>

<img width="1158" alt="qual_3" src="https://github.com/user-attachments/assets/a85f11d3-8170-4e19-95ac-0c4fec4ab282">
<p>Figure 7: Qualitative Comparison Example with Visualized Video Instruction Prompting by Llama2-7B-q4, (Task: How to make pancakes?).</p>

<img width="1158" alt="qual_4" src="https://github.com/user-attachments/assets/f1f033de-4466-4fda-91db-7d704f955ff4">
<p>Figure 8: Qualitative Comparison Example by VG-TVP (Ours), (Task: How to make pancakes?).</p>

<img width="1158" alt="gpt_qual" src="https://github.com/user-attachments/assets/2f6a09c0-b4c1-46f8-91c6-5e44c4a1ee33">
<p>Figure 9: Qualitative Comparison Example: GPT-3.5 vs. VG-TVP. Visuals (orange) are used to generate video plans.</p>


<h4>6. Survey Example</h4>
<img width="1158" alt="survey" src="https://github.com/user-attachments/assets/55833bdb-9eb5-4d05-9d10-c6288603c6b1">
<p>Figure 10: The survey example for the human evaluation study.</p>



<h4>7. References</h4>
<ol>
  <li>Lu, Y.; Lu, P.; Chen, Z.; Zhu, W.; Wang, X. E.; and Wang, W. Y. 2023b. Multimodal Procedural Planning via Dual Text-Image Prompting. CoRR, abs/2305.01795.</li> 
  <li>Lin, K. Q.; and Lei, S. W. 2023. VLog: Video as a Long Document. https://github.com/showlab/VLog. Accessed:2024-12-15.</li> 
  <li>Lu, Y.; Feng, W.; Zhu, W.; Xu, W.; Wang, X. E.; Eckstein, M. P.; and Wang, W. Y. 2023a. Neuro-Symbolic Procedural Planning with Commonsense Prompting. In The Eleventh International Conference on Learning Representations, ICLR, Kigali, Rwanda</li>
  <li>Chang, E. Y. 2023. Prompting Large Language Models With the Socratic Method. arXiv:2303.08769.</li>
  <li>Papineni, K.; Roukos, S.; Ward, T.; and Zhu, W.-J. 2002. BLEU: a method for automatic evaluation of machine translation. In Proceedings of the 40th Annual Meeting on Association for Computational Linguistics, ACL ’02, 311–318. USA: Association for Computational Linguistics.</li>
  <li>Banerjee, S.; and Lavie, A. 2005. METEOR: An Automatic Metric for MT Evaluation with Improved Correlation with Human Judgments. In Proceedings of the ACL Workshop on Intrinsic and Extrinsic Evaluation Measures for Machine Translation and/or Summarization, 65–72. Association for Computational Linguistics.</li>
  <li>Wang, J.; Yuan, H.; Chen, D.; Zhang, Y.; Wang, X.; and Zhang, S. 2023b. ModelScope Text-to-Video Technical Report. CoRR, abs/2308.06571.</li>
  <li>OpenAI. 2023. GPT-4 Technical Report. CoRR, abs/2303.08774.</li>
  <li>Jiang, A. Q.; Sablayrolles, A.; Mensch, A.; Bamford, C.; Chaplot, D. S.; de Las Casas, D.; Bressand, F.; Lengyel, G.; Lample, G.; Saulnier, L.; Lavaud, L. R.; Lachaux, M.; Stock, P.; Scao, T. L.; Lavril, T.; Wang, T.; Lacroix, T.; and Sayed, W. E. 2023. Mistral 7B. CoRR, abs/2310.06825.</li>
  <li>Touvron, H.; Martin, L.; Stone, K.; Albert, P.; Almahairi, A.; Babaei, Y.; Bashlykov, N.; Batra, S.; Bhargava, P.; Bhosale, S.; Bikel, D.; Blecher, L.; Canton-Ferrer, C.; Chen, M.; Cucurull, G.; Esiobu, D.; Fernandes, J.; Fu, J.; Fu, W.; Fuller, B.; Gao, C.; ...; and Scialom, T. 2023. Llama 2: Open Foundation and Fine-Tuned Chat Models. CoRR, abs/2307.09288.</li>
  <li>Tang, Y.; Ding, D.; Rao, Y.; Zheng, Y.; Zhang, D.; Zhao, L.; Lu, J.; and Zhou, J. 2019. COIN: A Large-Scale Dataset for Comprehensive Instructional Video Analysis. In Conference on Computer Vision and Pattern Recognition, CVPR, Long Beach, CA, USA, 1207–1216. IEEE.</li>
  <li>Zhukov, D.; Alayrac, J.; Cinbis, R. G.; Fouhey, D. F.; Laptev, I.; and Sivic, J. 2019. Cross-Task Weakly Supervised Learning From Instructional Videos. In Conference on Computer Vision and Pattern Recognition, CVPR, Long Beach, CA, USA, 3537–3545. IEEE.</li>


<h4>7. Citation</h4>

<p>If you found this repository useful, please consider citing our paper:</p>

```bibtex
@misc{ilaslan2024vgtvpmultimodalproceduralplanning,
      title={VG-TVP: Multimodal Procedural Planning via Visually Grounded Text-Video Prompting}, 
      author={Muhammet Furkan Ilaslan and Ali Koksal and Kevin Qinhong Lin and Burak Satar and Mike Zheng Shou and Qianli Xu},
      year={2024},
      eprint={2412.11621},
      archivePrefix={arXiv},
      primaryClass={cs.CV},
      url={https://arxiv.org/abs/2412.11621}, 
}


