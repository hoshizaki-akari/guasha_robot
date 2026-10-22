# AUTO-R38｜FAIRINO FR5刮痧机器人博士级完整研究报告

**日期：2026-10-21；内容验收：PASS_OFFLINE_RESEARCH_CONTENT_ONLY；真实FR5/D435/KWR75D/分段力台实验：0；E0–E5：HOLD；主学术Baseline：待定。**

**唯一核心问题：有限曝光/滤波窗口的平均测量能否支持瞬时刮板边缘载荷可信估计？** 研究与工程DEV轮次独立；本轮没有运行作者预训练模型，没有硬件运动，没有GitHub写入。

## 01｜上一轮归档与实质差分

R37归档在本轮实际可访问：ZIP有37个成员，CRC检查通过，SHA256SUMS记录36项逐项一致。报告、零基础讲义、候选矩阵、决策日志、`one_way_clock_gate.py`与78项测试已读取；R30–R37共355项测试重新执行。DEV-R11工程ZIP有890成员，CRC通过；本轮只读核查`one_way_evidence.py`和时间契约，未运行DEV工程，更未将DEV测试计入本轮。R37解决的是“设备事件与主机到达时间不能单向辨识延迟”；R38处理更深一层：即使真实采样中点已知，传感器输出也可能是时间窗口平均或滤波响应。上一轮的点时间配对不能自动变成点力真值。

**公式 F38-01（适用条件见本章）**

$$
\Delta_{38}=\{\text{support kernel},\text{rate bound},\text{row exposure},\text{independent truth}\}
$$

**表 T38-01**

| 维度 | 可审计证据或判断 |
|---|---|
|R37已证|单向offset/latency别名；共享时钟关联|
|R38新证|有限时间支持与瞬时值不等价；无速率界可隐藏尖峰|

**本章证据**：S38-01,S38-02；**决策**：D38-01,D38-02。

## 02｜决策问题、可证伪假设与入门Gate

Q1：有限宽曝光或力采样窗口的平均读数是否唯一决定窗口中心的瞬时接触力？Q2：有独立速率上界与有界时钟偏移时，多个平均读数能否在同一覆盖要求下比单窗口强基线缩小瞬时载荷区间？Q3：D435的RGB滚动快门行时刻与深度全局快门是否能用同一“帧时间点”代表？H1认为无速率界时不可辨识；H2认为假设盒内的线性规划必须覆盖真值；H3认为缺少RGB行延迟证书时拒绝跨行接触纠偏。证伪依赖独立标准器高速采样、真实曝光/滤波响应及已知时间偏移，而不是同一传感器的自一致性。

**公式 F38-02（适用条件见本章）**

$$
G_{38}=G_{clock}\land G_{kernel}\land G_{rate}\land G_{reference}\land G_{blind}
$$

**表 T38-02**

| 维度 | 可审计证据或判断 |
|---|---|
|入门|真实独立核、速率、参考证书齐全；仍只读|
|退出|失真/跨域/无独立真值时HOLD，撤销纠偏|

**本章证据**：S38-09,S38-10；**决策**：D38-02,D38-03。

## 03｜D435双快门与元数据时间语义

RealSense官方原D435规格区分立体深度global shutter与RGB rolling shutter；不能把D430系列的“全局快门”泛化到D435彩色流。官方`rs_frame.h`定义`SENSOR_TIMESTAMP`为曝光中点、`FRAME_TIMESTAMP`为读出/传输开始、`ACTUAL_EXPOSURE`为曝光宽度，且可能缺失。RGB每行采集时刻相差行延迟，M2画线识别在运动中会发生行间时间偏斜；M1/M3的RGB-D配准也不能只凭ROS相同header判断物理同时。目标设备固件、RGB行延迟和UVC metadata在本轮均未接入。

**公式 F38-03（适用条件见本章）**

$$
t_{r,\mathrm{center}}=t_{0,\mathrm{center}}+r\tau_{line}
$$

**表 T38-03**

| 维度 | 可审计证据或判断 |
|---|---|
|D435 stereo depth|global shutter；仍有有限曝光宽度|
|D435 RGB|rolling shutter；逐行中心时刻不同|
|ROS header|规范应为采集时间，但不是硬件认证|

**本章证据**：S38-03,S38-04,S38-05,S38-06；**决策**：D38-04。

## 04｜Ctrl-VIO全文：逐行时间校准已不是首创

核读Lang等2022公开全文III节：采用三次累计B样条描述SE(3)连续轨迹，逐行曝光时间进入视觉残差；滑动窗口采用边缘化策略。IV节在合成WHU-RSVI和真实TUM-RSVI、SenseTime-RSVI上比较RS-aware和GS方法，控制部分前端配置；作者设定真实序列B样条间隔0.05s、窗口11帧、最多150特征，逐行延迟校准在合成数据上有独立真值。其GPL-3.0代码依赖ROS Melodic/Ceres1.14，不是可直接搬到Humble的FR5控制包。该文足以反证“滚动快门行延迟在线校准首创”，但不提供刮板压力标签。

**公式 F38-04（适用条件见本章）**

$$
\mathbf T(t)=\operatorname{Spline}_{SE(3)}(\{\mathbf T_i\},t),\quad t_r=t_0+r\tau_{line}
$$

**表 T38-04**

| 维度 | 可审计证据或判断 |
|---|---|
|方法|连续时间B样条+行延迟+边缘化|
|实验|WHU合成与TUM/SenseTime真实VIO；APE真值|
|迁移|VIO/IMU≠D435外视接触载荷|

**本章证据**：S38-07,S38-08；**决策**：D38-05。

## 05｜Ma等全文：腕部F/T动态响应不能当瞬时真值

核读Ma、Song、Pan 2013全文2节的一阶传感器传递函数、后向差分动态补偿及移动平均低通；3节用自制两轴应变式传感器、绳索释放负阶跃和250kHz采集卡做实验。作者先扩展响应频带，再通过长度100的移动平均抑制补偿放大的高频噪声；论文报告响应调整时间由约4ms降至2.5ms。该实验是作者两轴传感器，不是KWR75D六轴F/T。方法说明滤波器会引入时间支持和群延迟，不能直接以某个ROS时间戳作为力发生时刻；对真实KWR75D需独立脉冲/阶跃标定。

**公式 F38-05（适用条件见本章）**

$$
y_k=\sum_{j=0}^{m-1}h_j f(t_k-\tau_j)+e_k,\quad \sum_jh_j=1
$$

**表 T38-05**

| 维度 | 可审计证据或判断 |
|---|---|
|作者设备|自制2轴F/T，250kHz采集，负阶跃|
|作者结果|约4ms→2.5ms调整时间；非FR5|
|R38要求|未知h_j或群延迟时拒绝瞬时力推断|

**本章证据**：S38-09；**决策**：D38-06。

## 06｜TaF-VLA全文与数据卡：重要新颖性反证

核读Huang等2026 arXiv v2全文III–VI：作者设计双平台机械同步压入装置，记录VBTS图像、ATI Axia80六轴F/T和12×12压电阵列，时序触觉编码使用ViT+因果Transformer并与力代码对比对齐；V节同数据、15次每任务真实评估，表I平均TaF-VLA64.8%、最强对照47.6%（作者任务，不是FR5）。作者数据卡实际公开MIT标记、10,053,265帧、3592 episodes、408序列、41.5GB；每帧列有`timestamp`，但未见独立曝光核/力积分核证明。论文VI节明确高频力尖峰时闭环频率受限。**因此“首次同步视觉/六维力/压力矩阵采集”已被直接反证。**

**公式 F38-06（适用条件见本章）**

$$
\mathcal D_{TaF}=\{I_t^{tac},\mathbf w_t,\mathbf P_t,\mathrm{timestamp}_t\}
$$

**表 T38-06**

| 维度 | 可审计证据或判断 |
|---|---|
|公开数据卡|10,053,265帧/3592 episodes/12×12+6D F/T|
|作者实验|TaF-VLA 64.8% vs 47.6%作者对照|
|缺口|无本轮parquet下载；timestamp不等于独立曝光/积分证书|

**本章证据**：S38-10,S38-11；**决策**：D38-07。

## 07｜Lei等2026全文：公平融合与平均力窗口

核读Lei等2026 arXiv v2全文III–V：同一Robomimic Diffusion Policy骨干上比较vision-only、拼接、torque gating、auxiliary goal、MoE及contact-gated CFG。作者三任务平均成功率：vision-only30%、单纯torque gating68%、提出方法82%，每个任务试验10或20次；作者公开MIT仓库README含Franka R3动作脚本和`mean_raw_ft --ft-mean-window 30`时间窗口平均选项。**这是一项源码接口审计反证**：现有多模态数据处理已把F/T当时间聚合量，且接触门控不是新颖首创；但README未给采样核物理计量证书，不能将窗口30直接换算为FR5毫秒。

**公式 F38-07（适用条件见本章）**

$$
\hat\epsilon=\hat\epsilon_v+w_{FT}(\hat\epsilon_{FT}-\hat\epsilon_v)
$$

**表 T38-07**

| 维度 | 可审计证据或判断 |
|---|---|
|作者公平条件|同骨干、同训练数据，融合模块不同|
|作者任务结果|torque gating68%；作者方法82%|
|源码接口|`mean_raw_ft`窗口平均；Franka R3≠FR5|

**本章证据**：S38-12,S38-13,S38-14；**决策**：D38-08。

## 08｜平均值不等于瞬时峰值：不可辨识证明

假设只有长度T的均匀窗口平均y，未知连续非负载荷f(t)的峰值不受独立变化率限制。构造宽度w、峰高H的三角脉冲，面积Hw/2；选择H=2yT/w即可保持平均y不变，令w趋近0则峰高无界。此反例不依赖统计噪声，属于观测算子本身的非单射性。多点离散样本若采样间隙内无速率上界也存在类似尖峰。不能从单一腕部wrench/平均值推断局部接触压力峰值，更不能把区间覆盖率当安全率。

**公式 F38-08（适用条件见本章）**

$$
y=\frac1T\int_0^T f(t)\,dt,\quad H=\frac{2yT}{w}\to\infty\ (w\to0^+)
$$

**表 T38-08**

| 维度 | 可审计证据或判断 |
|---|---|
|相同平均|不同脉冲宽度w和峰值H|
|物理含义|没有速率/带宽证书时峰值不可辨识|

**本章证据**：S38-09,S38-10；**决策**：D38-09。

## 09｜Lipschitz速率界：可证伪的充分条件

若已由独立高带宽参考验证任意时刻|f(t)-f(s)|≤L|t-s|，则均匀窗口平均与窗口中心瞬时值的偏差不超过L·T/4。若实际窗口中心与名义中心偏移至多u，另有|平均变化|≤Lu，故残差预算为e+L(T/4+u)。这里L的单位N/s，T与u必须转换为秒；若发生接触突变导致L失效，结论立即失效。与只依赖数据拟合的自置信区间不同，此为**有条件的确定性界**。

**公式 F38-09（适用条件见本章）**

$$
|f(t_0)-\bar f_{[t_0-T/2,t_0+T/2]}|\le LT/4
$$

**表 T38-09**

| 维度 | 可审计证据或判断 |
|---|---|
|速率界来源|独立非人体高带宽参考，不是本轮合成|
|核/时钟要求|BOX已证、T已证、|δ|≤u|
|失败|跳变或滤波核未知→HOLD|

**本章证据**：S38-09,S38-04；**决策**：D38-09。

## 10｜不确定时钟下的平均观测传播

令观测的真实窗口中心c+δ，|δ|≤u，传感器输出y=BOX_average(f;c+δ,T)+η，|η|≤e。由速率界，对同一f的两个平移窗口平均有|A(c+δ)-A(c)|≤L|δ|。因此只需对名义窗口的线性约束把误差膨胀Lu，就可容纳所有符合证书的真实中心；不需要凭空假设某个最可能延迟值。该误差扩张保守，尤其多个窗口共享同一时钟偏移时可能过宽；本轮暂不宣称做了相关时钟联合优化。

**公式 F38-10（适用条件见本章）**

$$
|y-A_{BOX}(f;c,T)|\le e+Lu
$$

**表 T38-10**

| 维度 | 可审计证据或判断 |
|---|---|
|不扩张|可能窄但失覆盖/LP冲突|
|扩张Lu|盒内覆盖有条件成立，区间更宽|
|未知核|不能只扩张时间，直接拒绝|

**本章证据**：S38-01,S38-04,S38-09；**决策**：D38-10。

## 11｜最小LP：连续分段线性真值与可执行约束

本轮独立实现`temporal_support_gate.py`，以整数ns时间网格的分段线性非负载荷f_i为变量，斜率约束|f_{i+1}-f_i|≤LΔt_i，力上限Fmax。每个BOX窗口的积分权重按线性插值**解析精确积分**，得到Σ_j w_ij f_j；结合e_i+Lu_i上下界形成LP。对目标时刻t*分别最小化、最大化插值后的f(t*)。代码不调用ROS2/SDK、不复制作者论文源码，使用SciPy HiGHS。网格模型仅在真实轨迹确实可由该分段线性表示时有严格覆盖证明；一般光滑曲线还需要独立插值误差预算，本轮没有虚称连续任意函数均精确。

**公式 F38-11（适用条件见本章）**

$$
\min/\max\ \mathbf q^T\mathbf f\ \mathrm{s.t.}\ |f_{i+1}-f_i|\le L\Delta t_i,\ |\mathbf w_j^T\mathbf f-y_j|\le e_j+Lu_j
$$

**表 T38-11**

| 维度 | 可审计证据或判断 |
|---|---|
|变量|每个网格节点的法向载荷N|
|强约束|非负、上限、速率、时间窗口平均|
|输出|[下界,上界] + HOLD；无动作接口|

**本章证据**：S38-17,S38-09；**决策**：D38-10。

## 12｜解析积分、整数纳秒与独立数值判据

对任意网格小段[a,b]，线性插值f(t)=(1-λ)f_a+λf_b，窗口交集[l,r]的右节点积分权重为((r-a)^2-(l-a)^2)/(2(b-a))，左节点权重为(r-l)减右权重；最后除窗口宽T。权重非负且总和为1。代码先对巨大Unix ns减去整数anchor，再转浮点秒，避免超过2^53时直接浮点损失纳秒；严格拒绝奇数ns窗口（避免半ns未定义）、越界窗口、重复观测、非有限值、时钟/标定/episode不一致。测试覆盖非均匀网格解析线性函数积分和巨大时间戳平移不变性。

**公式 F38-12（适用条件见本章）**

$$
w_{right}=\frac{(r-a)^2-(l-a)^2}{2(b-a)T},\quad w_{left}=\frac{r-l}{T}-w_{right}
$$

**表 T38-12**

| 维度 | 可审计证据或判断 |
|---|---|
|积分真值|仿射函数解析平均，独立于LP求解|
|时间数值|整数ns先相减再换秒|
|拒绝项|错误核、版本、单位、NaN、窗口越界|

**本章证据**：S38-17,S38-01；**决策**：D38-10。

## 13｜公平基线：同一人工真值、同一输入、同一拒绝率

本轮预注册四种方法：弱基线将最近平均值误作瞬时值，仅加读数误差；强基线使用**一个**BOX窗口的解析Lipschitz最坏界（含中心差与u），对全部窗口取最窄；消融LP使用多个窗口但故意不膨胀时钟误差；完整LP使用同样多个窗口并膨胀Lu。四法使用完全相同的120条分段线性人工真值轨迹、同5个窗口、同误差盒与同目标时刻。拒绝单独计数，不删除其他方法的相同episode；比较覆盖、平均区间宽度及拒绝率，不以更窄而失覆盖的算法为胜。

**公式 F38-13（适用条件见本章）**

$$
W=U-L,\quad C=\frac{\#\{f^*\in[L,U]\}}{N},\quad R=\frac{\#\mathrm{HOLD}}N
$$

**表 T38-13**

| 维度 | 可审计证据或判断 |
|---|---|
|弱点估计|平均当瞬时：73/120覆盖、宽0.016N|
|单窗强基线|120/120、宽0.46768N|
|完整多窗LP|120/120、宽0.43805892N|

**本章证据**：S38-17；**决策**：D38-11。

## 14｜消融结果、超证书漂移与尖峰失败

固定种子380038、网格5ms、L=12N/s、时间中心证书u=6ms。120组正常人工数据：弱点估计73/120覆盖；强单窗120/120、平均宽0.46768N；多窗LP不做时间扩张106/120覆盖且7/120不可行；完整多窗LP120/120、平均宽0.43805892N。完整LP相对强单窗平均收窄约6.33%，是**同假设盒内的数学收益**。60组隐藏超预算时间漂移中完整LP43/60覆盖、17/60拒绝；40组故意违反速率界的窄尖峰真实峰值0/40被覆盖。未知漂移/速率不能靠内部拟合自动发现。

**公式 F38-14（适用条件见本章）**

$$
\rho=1-\frac{0.43805892}{0.46768}\approx0.0633
$$

**表 T38-14**

| 维度 | 可审计证据或判断 |
|---|---|
|正常盒内|强基线120/120；完整LP120/120|
|漏时间扩张|106/120覆盖，7/120拒绝|
|错误假设|超预算17/60拒绝；超速尖峰0/40覆盖|

**本章证据**：S38-09,S38-10；**决策**：D38-11。

## 15｜工具几何、法兰/TCP/接触线与空间真值

时间窗口推断的对象是“法向合力在某时刻的可能范围”，绝不等于有限宽刮板各边缘局部压力。法兰位姿、经标定TCP位姿与柔性工具实际接触中心/接触线不同；接触线上的偏载、未知纯偶矩、多点分布、切向摩擦可使同一腕部合力/力矩对应多种压力场。真实I-A仍需独立分段力台、工具边缘和手眼标定、力传感器偏置/重力/动态补偿。R38时间核算法只解决“这些测量代表哪段时间”，不越界宣称空间反演已解决。

**公式 F38-15（适用条件见本章）**

$$
\mathbf w_{wrist}=\int_{\Gamma}\!\begin{bmatrix}\mathbf f(s)\\\mathbf r(s)\times\mathbf f(s)\end{bmatrix}ds+\mathbf w_{couple}
$$

**表 T38-15**

| 维度 | 可审计证据或判断 |
|---|---|
|空间真值|分段非人体力台+独立几何|
|无法唯一|未知纯偶矩/摩擦/多点分布|
|失败回退|拒绝接触姿态纠偏、请求已认证卸载路径|

**本章证据**：S38-10,S38-15；**决策**：D38-07,D38-12。

## 16｜主论文竞争、唯一创新与相邻2026覆盖

五篇候选保持VPEC、ViPER、FEATS、ImplicitRDP、RDP的投入优先级，详见完整REGISTER：每篇作者、年份、DOI、许可/数据/代码、公式、速率、FR5迁移与最小补证已逐项列出。TaF-VLA2026拥有同步多模态真实阵列数据但代码项目页仍显示coming soon，且其内视触觉与D435外视不同，暂作数据/方法监视而非强行替换主论文。Lei2026和FoAR2025已覆盖宽泛接触门控，Ctrl-VIO已覆盖滚动快门时间校准。**唯一条件性首选I-A**收紧为“有限宽刮板部分可辨识+独立物理真值+经认证的时间支持核+失败拒绝”；唯一备选I-B仍为延迟鲁棒换向卸载控制。

**公式 F38-16（适用条件见本章）**

$$
I_A=\text{partial-identification}\cap G_{reference}\cap G_{kernel}\cap G_{rate}
$$

**表 T38-16**

| 维度 | 可审计证据或判断 |
|---|---|
|主论文|5篇竞争；赢家待定、排序未变|
|新颖性修订|同步力标签、时序编码、接触门控均非首创|
|备选|I-B：WORK/UNLOAD/离面返程延迟鲁棒|

**本章证据**：S38-10,S38-11,S38-12,S38-14；**决策**：D38-07,D38-12。

## 17｜M1/M2/M3三入口与统一三维曲线

M1继续支持人工有序三维点与指定路线；M2识别背部实体画线、要求用户确认起点终点和方向，并输出有序三维曲线；M3从视觉解剖标志生成候选穴位与可信可编辑关键点图，由专家审核后允许选择两点路线或审核过的刮痧模板。三者统一输出位置、切向、曲面法向、置信/不确定度、工具姿态、连续有限宽扫掠资格、WORK/UNLOAD/OFFSURFACE_RETURN模式，接触前人工审核。R38不改上层路径代码，不占用博士创新份额；新增的逐行曝光Gate将来应在M2彩色线识别和M3 RGB-D关键点配准之前执行。

**公式 F38-17（适用条件见本章）**

$$
\gamma(s)=(\mathbf p,\mathbf t,\mathbf n,\Sigma,\mathbf R_{tool},G_{sweep},m)
$$

**表 T38-17**

| 维度 | 可审计证据或判断 |
|---|---|
|M1|人工有序点与路线|
|M2|实体线、起终方向确认|
|M3|专家审核点图/模板/两点路径|

**本章证据**：S38-03,S38-06；**决策**：D38-04,D38-12。

## 18｜安全、工程强Baseline与ROS2边界

FR5厂家FT_Control恒力+平滑UNLOAD+OFFSURFACE_RETURN仍是规划中的工程强Baseline，未连接SDK v2.3.0/robot v4.0.0或核固件返回码、实际外环频率。KWR75D动态补偿、D435曝光元数据、手眼/边缘标定、非人体仿体独立力台均未取得。ROS2 Humble `Image.header.stamp`规范要求采集时间，但不能证明真实曝光中点；本轮所有程序只读、纯数学、输出HOLD_NO_MOTION，不发布动作或调用FT_Control。物理实验顺序仍E0标定→离面轨迹→低能量单向接触→平滑卸载→离面返程；贴面返程与真人必须另行审查。

**公式 F38-18（适用条件见本章）**

$$
G_{motion}=0\quad\text{for all AUTO-R38 outputs}
$$

**表 T38-18**

| 维度 | 可审计证据或判断 |
|---|---|
|工程Baseline|厂家恒力+卸载+离面返程；未运行|
|ROS2|只读采集时间语义，不等于曝光认证|
|安全|E0–E5 HOLD；真人另行风险/伦理审批|

**本章证据**：S38-06,S38-04,S38-09；**决策**：D38-03,D38-12。

## 19｜局限、独立真值计划与下一轮最小解法

R38的LP对“网格上分段线性、非负、已知BOX核、真实L、真实u、真实误差界”有条件覆盖；对真实连续曲线的离散误差、未知IIR核、RGB滚动快门行延迟变化、未知纯接触力矩、力台跨通道串扰、非人体组织材料变化均无物理证明。下轮优先只读核验TaF-Dataset一个小型parquet/metadata样本的timestamp/单位/episode划分与压力矩阵计量字段（需合法下载）；再建立D435真实曝光元数据和KWR75D动态阶跃/滤波核的独立标准器证据模板。若没有设备，提交可验证缺口，不升级为实体Gate。当前不访问用户私有GitHub，不新建仓库，所有测试人工。

**公式 F38-19（适用条件见本章）**

$$
G_{exit}=\neg(G_{kernel}\land G_{rate}\land G_{truth})\Rightarrow\mathrm{HOLD}
$$

**表 T38-19**

| 维度 | 可审计证据或判断 |
|---|---|
|可立即执行|只读数据卡/小样本字段审计、合成故障回归|
|需要硬件|同步脉冲、标准力台、KWR75D阶跃/动态响应|
|退出|无法独立验证核与L时停止点力纠偏|

**本章证据**：S38-05,S38-11,S38-17；**决策**：D38-11,D38-12。

## 附录A｜逐项来源与获取层级（完整镜像）

# AUTO-R38｜逐项来源审计（2026-10-21，去重17项）

**层级定义**：A=本轮公开全文中方法/实验/限制均实际阅读；B=本轮官方产品、API、源码README、数据卡或摘要实际打开；LOCAL=本地文件实际读取；H=沿用前轮且本轮未重新阅读的来源。没有任何作者模型被运行。论文、代码、数据许可证分开判断。

|ID|作者/机构、作品、年份、DOI或版本|访问层级与实际范围|限制及迁移风险|实际URL/本地路径|
|---|---|---|---|---|
|S38-01|AUTO-R37研究归档（2026）|LOCAL：37成员、36项SHA、CRC；报告、讲义、日志、矩阵、代码、测试实读，重跑R30–R37测试|不等于实体复现|`/mnt/data/guasha_autonomous_round37/AUTO_R37_REPRODUCIBLE_PACKAGE.zip`|
|S38-02|DEV-R11工程归档（2026）|LOCAL：890成员、CRC；只读核查`one_way_evidence.py`及时间契约|未运行DEV工程，未修改DEV代码|`/mnt/data/fr5_dev_release_DEV_R11/FR5_GUASHA_DEV_R11_COMPLETE.zip`|
|S38-03|RealSense官方D435产品技术规格（访问2026-10-21）|B：搜索缓存原D435技术表明确立体深度global shutter、RGB rolling shutter；现官网重定向至D430系列页|产品页面改版，目标D435具体固件、模式与行延迟未实测|https://www.realsenseai.com/products/stereo-depth-camera-d435/|
|S38-04|RealSense `rs_frame.h`官方API（master，访问2026-10-21）|B：`SENSOR_TIMESTAMP`曝光中点、`FRAME_TIMESTAMP`读出开始、`ACTUAL_EXPOSURE`曝光宽度、timestamp domain枚举|master非固定commit；不能证明用户WSL可读全部metadata|https://github.com/realsenseai/librealsense/blob/master/include/librealsense2/h/rs_frame.h|
|S38-05|RealSense官方Frame Metadata指南（访问2026-10-21）|B：元数据需设备固件与OS/UVC支持，必须先`supports_frame_metadata`|历史Linux内核说明未覆盖WSL22.04实际环境|https://dev.realsenseai.com/docs/frame-metadata/|
|S38-06|ROS2 Humble `sensor_msgs/msg/Image.msg`官方仓库|B：`header.stamp`应为图像采集时间，`frame_id`为光学坐标|规范性约定不保证任何驱动实现满足|https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg|
|S38-07|Lang等，*Ctrl-VIO: Continuous-Time Visual-Inertial Odometry for Rolling Shutter Cameras*，RA-L 2022，arXiv:2208.12008|A：全文III方法、IV实验、V局限，核读B样条、逐行时间、WHU/TUM/SenseTime和消融|VIO+IMU≠RGB-D刮板压力，未运行代码|https://arxiv.org/html/2208.12008|
|S38-08|Ctrl-VIO作者仓库，GPL-3.0，README（访问2026-10-21）|B：ROS Melodic、Ceres1.14、OpenCV3.3、GPL-3.0，未固定commit|ROS1接口与ROS2 Humble不同；不复制其源码|https://github.com/APRIL-ZJU/Ctrl-VIO|
|S38-09|Ma、Song、Pan，*Dynamic Compensation for Two-Axis Robot Wrist Force Sensors*，Journal of Sensors 2013，DOI 10.1155/2013/357396|A：全文2节模型/差分补偿、3节步阶台架/滤波对照、4节结论|作者自制2轴传感器250kHz采样、τ约1ms，不得当KWR75D规格|https://onlinelibrary.wiley.com/doi/10.1155/2013/357396|
|S38-10|Huang等，*TaF-VLA: Tactile-Force Alignment...*，arXiv:2601.20321v2（2026）|A：全文III数据装置、IV历史触觉编码、V公平实验、VI失效；arXiv DOI 10.48550/arXiv.2601.20321|内置VBTS非外部D435；作者成功率不可转FR5|https://arxiv.org/html/2601.20321|
|S38-11|作者TaF-Dataset Hugging Face数据卡（访问2026-10-21）|B：MIT标签、10,053,265帧/3592 episodes/408序列、12×12压力+6D F/T+图像，时间戳字段；41.5GB|**仅核数据卡未下载parquet或视频**；单一timestamp字段不能证明独立曝光/积分时间|https://huggingface.co/datasets/jiamig/taf-dataset|
|S38-12|Lei等，*Learning When to See and When to Feel*，arXiv:2604.01414v2（2026）|A：全文III方法、IV三任务公平消融、V限制；自适应门控与未来力矩辅助对照|Franka R3非FR5；82%作者任务成功率不可迁移|https://arxiv.org/html/2604.01414|
|S38-13|Lei等作者代码仓库（2026）|B：README、MIT声明、Franka R3 rollout、HDF5时间同步与`mean_raw_ft`选项|未固定commit、未下载运行；真实FR5动作接口不兼容|https://github.com/RollingOat/vision-torque-fusion-for-contact-aware-manipulation|
|S38-14|FoAR，RA-L 2025，DOI 10.1109/LRA.2025.3560871|B：IEEE摘要、2025-04-14发表、未来接触预测与视觉F/T动态融合|未取得全文；不能把其性能当FR5|https://ieeexplore.ieee.org/document/10964857/|
|S38-15|Li与Thuruthel，*Grasp Independent Indirect Tool Force Estimation using Vision-based Tactile Sensors*，RA-L 2025，DOI 10.1109/LRA.2025.3629985|B：UCL机构页、作者接受稿下载入口及摘要|本轮未下载全文；工具力估计已邻近覆盖，但不是刮板有限宽真值|https://discovery.ucl.ac.uk/id/eprint/10217859/|
|S38-16|Chen等，*UniForce*，arXiv:2602.01153（2026）|B：官方摘要；跨触觉传感器潜在力空间与准静态平衡|未核本轮全文；准静态条件不能直接用于动态刮痧|https://arxiv.org/abs/2602.01153|
|S38-17|SciPy/HiGHS官方项目与许可（访问2026-10-21）|B：SciPy BSD-3-Clause、`scipy.optimize.linprog`，本轮调用安装版本而非复制作者实现|求解器正确≠物理假设正确；版本须记录|https://github.com/scipy/scipy|

**访问受限记录**：MDPI *Short-Transient Discrete Time-Variant Filter...* 2020（Electronics 9:1291）原站429；只看检索索引摘要/方法片段，不计A，不列入上述17项已实读来源。ROS2 docs.ros.org Image页面返回Anubis拒绝，改用官方Humble仓库定义。`TaF-Dataset`未下载41.5GB，不声称做了真实数据统计。

## 附录B｜完整主论文Baseline与创新候选矩阵（完整镜像）

# AUTO-R38｜BASELINE_AND_INNOVATION_REGISTER（跨轮可追溯）

**截至2026-10-21：主学术论文赢家=待定；排序仍为VPEC→ViPER→FEATS→ImplicitRDP→RDP。** 排名只代表合法复现资格审计的下一步投入优先级，绝不是FR5实测性能排名。R37→R38名次均未改变；**创新问题表述已收紧**。每项标注A=本轮全文、B=本轮官方摘要/仓库、H=历史登记、U=尚未核。此轮未下载、训练或运行下列五篇作者代码。

|优先级、作者/题名/年份/DOI|本轮访问级别；源码、数据、许可、版本与复现参数|数学与接口、FR5/D435/KWR75D/Ubuntu22.04/ROS2/GPU速率匹配|2025–2026反证、非人体真值、判断、变化与最小补证|
|---|---|---|---|
|**1 VPEC**：Patrick Grady、Jeremy A. Collins、Samarth Brahmbhatt、Christopher Twigg、Shuran Tang、James Hays、Charles C. Kemp；*Visual Pressure Estimation and Control for Soft Robotic Grippers*；IROS 2022；**10.1109/IROS47612.2022.9982073**|H：R37曾核作者MIT代码仓库，Python3.10/PyTorch1.12.1，约170GB；本轮未重新访问仓库、论文全文或数据；数据许可、固定commit、权重SHA=U。|外部RGB→软夹爪压力图、闭环视觉；原Stretch与D435眼在手/曲面柔性刮板不同。输入曝光宽度、图像标签时间对齐、工具遮挡、实际外环频率=U。无需预设FR5底层关节力矩，但SDK控制周期未知。|TaF-VLA2026已公开同步图像/6D力/12×12阵列数据，直接否定“同步力数据首次提出”；VPEC仍**优先做只读合法小样本复现资格审计**。非人体真值须独立校准；最小补证=作者仓库commit、数据授权、标签采集核与时间真值。名次不变。|
|**2 ViPER**：Jeremy A. Collins、Cody Houff、Patrick Grady、Charles C. Kemp；*Visual Contact Pressure Estimation for Grippers in the Wild*；IROS 2023；**10.1109/IROS55552.2023.10342124**|H：R37作者README提供train/test/weak分组，仓库根未显示LICENSE，故复制/改编权限U；OneDrive数据/权重许可和SHA=U。|眼在手RGB预测压力分箱、Sensel阵列+腕F/T弱监督；原Stretch非FR5。数据帧和阵列积分/滤波时间不明，9-bin映射单位、深度输入及真实控制带宽U。|TaF-VLA更强地占据通用视觉触觉+力标签空间；**有条件保留**，许可未明时不运行/改编。最小补证=正式许可、完整方法、标签时间核与分段仿体真值。名次不变。|
|**3 FEATS**：Erik Helmut、Luca Dziarski、Niklas Funk、Boris Belousov、Jan Peters；*Learning Force Distribution Estimation for the GelSight Mini Optical Tactile Sensor Based on Finite Element Analysis*；IROS 2025；**10.1109/IROS60139.2025.11246486**|H：R35历史核作者MIT代码和数据卡、约3.98GB、Python3.11.10；本轮未重新核版本hash/权重/许可证原文，U。|GelSight Mini内视触觉+FEA标签，非外部D435；材料模型与压力真值的独立性是缺口；ROS2/FR5力控速率不依赖其模型，但实际相机频率、输出标定U。|TaF-VLA2026 12×12压力图和多传感器时序对照进一步缩小通用力分布学习新颖性；**保留方法对照**，不先购买不匹配硬件。最小补证=模型/数据许可证、独立阵列计量、动态时间支持。名次不变。|
|**4 ImplicitRDP**：Wendi Chen、Han Xue、Yi Wang、Fangyuan Zhou、Jun Lv、Yang Jin、Shirun Tang、Chuan Wen、Cewu Lu；*ImplicitRDP: An End-to-End Visual-Force Diffusion Policy with Structural Slow-Fast Learning*；2025 arXiv/2026作者RA-L信息；**10.48550/arXiv.2512.10946**（正式DOI U）|H：历史公开全文/作者页；代码/数据/权重许可证、commit、SHA=U；不执行不可信pickle。|慢视觉快6D力、虚拟目标；原Flexiv非FR5。pose9→6、action19→13、力坐标、外环周期与动作许可未核；双RTX3090不代表实时或安全。|Lei2026的相同骨干公平融合研究说明接触门控不是新颖通用创新；**有条件保留但实体暂缓**。最小补证=代码许可、只读离线前向、厂家SDK反馈周期。名次不变。|
|**5 RDP**：Han Xue、Jieji Ren、Wendi Chen、Gu Zhang、Fang Yuan、Guoying Gu、Huazhe Xu、Cewu Lu；*Reactive Diffusion Policy: Slow-Fast Visual-Tactile Policy Learning for Contact-Rich Manipulation*；RSS 2025；**10.15607/RSS.2025.XXI.052**|H：R37正式RSS页面已核作者与DOI；本轮未重新下载；代码/数据/权重许可、版本、SHA=U。|慢视觉与快触觉；作者硬件、触觉信号/示教与FR5腕六维F/T不等价；不确定是否需要不可用低层力矩/高频servo。|2026 TaF-VLA/Lei等已有历史触觉编码和接触门控，**暂缓移植**；最小补证=作者代码许可证、输入输出动作接口、独立力台验证。名次不变。|

## 新增相邻论文审查：为何不立即升为主Baseline

- **TaF-VLA**（Huang等2026，DOI **10.48550/arXiv.2601.20321**）：本轮A全文、作者数据卡B；MIT标记、41.5GB、10,053,265同步帧、3,592 episodes、12×12压力阵列+6D F/T+视触觉图像；**源码项目页仍显示Code Coming Soon**，预训练权重/固定commit未知。它是非常有价值的**独立非人体外部数据可用性候选**，但视触觉相机看的是硅胶内部形变，不是D435从外部看刮板边缘；单`timestamp`字段没有曝光核/滤波核证据。决定**加入监视名单而非替代主论文五选一**；下轮可在授权允许时仅下载小型metadata/parquet片段，核验时间与单位。
- **Lei等2026**（arXiv:2604.01414v2）：本轮A全文+作者MIT代码B，Franka R3、Robomimic diffusion，同骨干对照三任务；作者接触门控68%/作者方法82%平均任务成功率，**不等于FR5**。其代码的`mean_raw_ft`窗口平均明确提示**力读数可能是时间窗口聚合量**，不能当瞬时点力。研究问题定位于接触融合，不满足本项目有限宽工具独立真值，因此不升为主Baseline。
- **FoAR 2025**（DOI 10.1109/LRA.2025.3560871）：只读IEEE摘要，未来接触预测门控已覆盖宽泛创新；源码/许可证/参数未核。
- **Ctrl-VIO 2022**（arXiv:2208.12008）：A全文，作者GPL-3.0、ROS Melodic；逐行rolling shutter校准已成熟，非刮板力估计主论文。

## R37→R38实质差分及唯一创新

R37证明`arrival time`无法单独识别曝光事件，必须有独立延迟证书。**R38再证明：即使采样中点完全正确，时间平均/滤波测量也不等于中点瞬时载荷；若没有独立速率上界，有限平均可隐藏任意窄尖峰。** 还新增D435 RGB滚动快门与立体深度全局快门分流Gate。R38的120组合成同条件中强单窗口Lipschitz基线覆盖120/120、平均宽0.46768N，多窗口LP覆盖120/120、平均宽0.43805892N；无时间扩张LP仅106/120覆盖、7/120拒绝。**这些只是模型盒内数学案例，不能用于物理安全率或论文性能。**

**唯一条件性首选I-A**：有限宽刮板边缘载荷部分可辨识区间 + 独立计量分段载荷真值 + 可认证的视觉/六维力**时间支持核**与失败拒绝。腕部合力与力矩不能唯一反演未知纯偶矩/多点压力/摩擦；只允许在几何、工具、传感器、标定、时间、速率、真实物理真值均合格后谈收益。**本轮新增G_support_kernel与G_force_rate_truth**。若真实仿体的同覆盖/拒绝公平对照不能超越wrench-only LP，或时钟/滤波核无法独立认证，I-A退出或降级。

**唯一备选I-B**：接触换向WORK→UNLOAD→OFFSURFACE_RETURN的延迟鲁棒柔顺控制；待厂家FT_Control/外环只读时序、非人体力台参考和能量/过冲门限获得独立批准后再审。摩擦扰动分离、RGB-D姿态纠偏列为监视，不堆EKF/ESO/MPC。

**工程强Baseline**：厂家FT_Control许可的恒力控制+平滑UNLOAD+离面返程，**未核真实SDK/固件/力传感器补偿、未接硬件**；离线wrench-only LP是数学强基线，不能混称厂家实测。

**全系统入口不缩减**：M1有序人工3D点；M2实体画线/起终方向人工确认；M3解剖标志/专家审核穴位候选/可编辑点图/两点或已审模板。统一输出位置、切向、曲面法向、置信区间、工具姿态、有限宽连续扫掠资格及WORK/UNLOAD/OFFSURFACE_RETURN；接触前人工审查。法兰、TCP、柔性工具真实接触线分开标定。硬件运动默认拒绝，真人测试需独立伦理/风险审查。仅浏览公开官方仓库网页；没有访问用户私有GitHub仓库、写入或新建仓库。

## 附录C｜完整决策日志（D38-01至D38-12）

|决策ID|本轮判断|
|---|---|
|D38-01|R37完整ZIP与内部SHA/CRC实核；R30–R37测试重新运行，DEV-R11只读核不计测试。|
|D38-02|科学问题从“时钟点配对”收紧到“有限时间支持平均值能否界定瞬时接触力”。|
|D38-03|G_kernel/G_rate/G_reference/G_blind缺一HOLD；HOLD不代表机器人已卸载。|
|D38-04|D435 RGB rolling shutter与stereo depth global shutter分开；缺行延迟证书拒绝M2/M3时序纠偏。|
|D38-05|Ctrl-VIO已有逐行延迟校准，时序B样条不作为本项目主创新。|
|D38-06|Ma等动态F/T滤波研究反证“传感器读数天然瞬时”；KWR75D动态核未核。|
|D38-07|TaF-VLA2026同步图像/6D力/压力阵列数据公开，否定泛化的同步数据首创；加入监视不升主论文。|
|D38-08|Lei2026同骨干公平门控/源码窗口平均，接触门控不作为独创；不运行Franka代码。|
|D38-09|无L时三角脉冲同平均不同峰值；L必须独立计量，不能从本模型自证。|
|D38-10|采用BOX窗口解析积分+Lipschitz+Lu误差传播的最小LP；所有输出HOLD。|
|D38-11|同条件强单窗120/120、完整LP120/120但仅6.33%收窄；错证书/尖峰明确退出。|
|D38-12|五主论文排序不变、赢家待定；唯一I-A收紧、唯一I-B备选；E0–E5与真人HOLD。|

## 附录D｜实际可复现文件与内容验收

执行`python -m pytest -q tests/test_temporal_support_gate.py`与`python ablation_r38.py`；`python run_prior_regressions_r38.py`仅重跑实际嵌套归档R30–R37测试。完整JSON/JUnit、源码SHA和QA见ZIP；**所有代码输出HOLD，绝无机器人运动接口**。