# Cooperative MIMO-ISAC System with Interference Rejection Beamforming for Unmanned Aerial Vehicles [with MATLAB Code]

1. Joint Transmit Beamforming for Multiuser MIMO Communications and MIMO Radar
2. Research Background: Frequency Spectrum Crowding in Radar and Communication, Integrated Sensing Becomes Key to Breakthrough
3. Author Team: Liu Xiang, Huang Tianyao, Nir Shlezinger, Liu Yimin, Zhou Jie, Yonina C. Eldar
4. Research Background: The proliferation of wireless communication services has led to a significant increase in traditional communication frequency spectrum resources; however, military and vehicle-mounted radars have extensively occupied the S and C bands below 10GHz, causing severe interference between the two systems due to their adjacent frequency bands. Early solutions involved allowing separate devices for radar and communication to coexist with shared spectrum, relying on opportunities for spectrum access, zeroing of interference, adaptive interference cancellation, and beam optimization to coordinate their coexistence. However, these solutions required inter-device communication channels, radar waveform parameters, and the establishment of centralized scheduling units, presenting high hardware and algorithmic complexity.
5. In scenarios such as vehicle and aircraft, where radar perception and data transmission share a single transmitting platform, dual-function radar communication integration (DFRC) technology has emerged, offering natural advantages over separate devices: sharing an RF frontend and antenna aperture reduces hardware costs, volume, weight, and eliminates additional coordination overhead. However, there are still two major challenges in integrated design:
6. Hardware Level: The vast differences in working ranges and power requirements between radar and communication amplifiers;
7. Algorithmic Level: How to design a unified transmit signal that can simultaneously accommodate multi-user communication and multi-target radar detection.
8. Previous integrated solutions can be divided into two categories, but each has significant shortcomings:
1. The mono-antenna integrated waveform scheme includes ideas such as time-division multiplexing integrated, OFDM communication waveform multiplexing radar, LFM radar superimposing CPM modulation, etc. Its shortcomings are very prominent: it can only form a single beam and cannot simultaneously illuminate multiple radar targets or serve multiple communication users; if the communication receiving end is not within the main lobe of the radar, the received signal-to-noise ratio will significantly drop, and after waveform fusion, the sidelobes of the radar will rise, which will damage the detection performance.
2. MIMO Array Integrated Solution (Divided into two technical routes) 1) Information Encapsulation Type: Mainly radar-based, with communication bits embedded in the sidelobe amplitude, phase, antenna index, and carrier frequency of the radar. The disadvantage is that the single pulse can carry very few communication symbols and the transmission rate is extremely low. 2) Beamforming transmission: Utilizing the freedom of the airspace to generate multiple beams, serving both users and target illumination simultaneously. Previous representative studies only reused communication waveforms for radar detection, and the spatial degrees of freedom available in MIMO radar equals the number of communication users. Once the number of users is less than the number of transmitting antennas, the degree of freedom of radar beam shaping is insufficient, and the beam distortion is serious. Moreover, the optimization model is non-convex and can only obtain suboptimal solutions.
In response to the existing issues of limited freedom and significant radar performance degradation, this paper introduces a novel joint beamforming framework. It also independently precodes radar-specific waveforms and communication symbols, maximizing the utilization of array spatial freedom and balancing communication service quality with radar detection capabilities.
III. Core Summary of the Paper
The system emits a weighted superposition signal of independent radar waveforms and communication symbols, which respectively forms multi-directional beams towards the radar target and communication receiver. Transform the design of weight coefficients (pre-coding matrices) into an optimization problem: Optimize the MIMO radar transmission beam performance while ensuring that each communication user's Signal-to-Interference-plus-Noise Ratio (SINR) exceeds a preset threshold.
Although the original optimization model is non-convex, the paper proves that this problem can be transformed into an equivalent convex optimization problem through semidefinite relaxation (SDR), with the relaxation solution being strictly tight (the relaxed solution equals the optimal solution of the original problem). To reduce computational overhead, a low-complexity design based on zero forcing (ZF) is further proposed to directly eliminate user interference and radar communication interference.
Unlike previous work that relied solely on the synthesis of communication symbols to generate radar beams, this paper's scheme provides MIMO radar with complete spatial freedom, significantly enhancing its detection performance. Simulation verification shows that under reasonable communication quality constraints, the radar performance of this integrated scheme can approach that of a pure radar single-function system.
IV. Core Method Overview (I) System Signal Model: The transmitter is equipped with an M-element co-located uniform linear array, which simultaneously accomplishes single-base station MIMO radar detection and downlink multi-user communication. The transmitted signal is a weighted sum of two independent signals:
M: Independent radar pseudorandom waveform;: Radar pre-coding matrix.
K-way parallel communication symbol stream (serving K single-antenna users);: Communication precoding matrix.
The system satisfies four basic assumptions: radar, zero mean of communication signals, time-domain stationarity. The two are unrelated; Different users communicate symbols orthogonally; The radar waveforms from various sources are uncorrelated. The transmitting antenna adopts a single-antenna equal power constraint, with a fixed total transmitting power. All antennas have an equal transmitting power, meeting the engineering requirements of radar hardware full-power transmission.
(II) Two sets of performance evaluation indicators
2. The performance metrics for multi-user communication are constrained by the user fair SINR, requiring all users to receive an SINR not lower than a threshold. The communication reception signal includes three types of interference: other user communication interfering, detection interference from radar waveforms, and Gaussian noise. The higher the SINR threshold, the stronger the constraints on the precoding matrix, leading to greater radar performance loss.
(III) Two joint beamforming solution algorithms:
Algorithm 1: Global optimal solution based on semidefinite relaxation (SDR)
Original optimization problem: Minimize the radar beam loss function, with constraints including antenna power equality, all user SINR lower bounds; the model is non-convex due to the rank 1 constraint and is difficult to solve directly.
2  
Relaxation transformation: Remove the rank 1 constraint and convert it into a convex quadratic semidefinite programming (QSDP), which can be solved in polynomial time using tools like CVX.
3  
Theoretical proof: The relaxation has tightness, and the convex optimization solution can definitely recover the original precoding matrix that meets the rank 1 conditions, achieving global optimality.
4  
Advantages and Disadvantages: Optimal radar performance; however, the computational complexity is high, growing exponentially with the number of users K at a rate of 6.5.
Algorithm 2: Low-complexity suboptimal solution based on zero-forcing (ZF)
Core idea: Forcefully eliminate all interference, making the radar signal arrive at the user channel zero and the communication between users diagonalizable, completely cancelling out user interfering and radar interference.
2  
Model transformation: Convert the interference elimination constraint into a linear equality constraint of the covariance matrix, also converted into a convex QSDP for solving.
3  
Advantages and Disadvantages: Significantly reduced computational complexity, only related to the number of transmitting antennas M, without any high-order terms associated with the number of users; under high SINR constraints, the performance approaches the SDR optimal solution; under low SINR scenarios, forcing zeros can excessively limit array freedom, resulting in poorer radar performance compared to the SDR.
(IV) Simulation Key Conclusions Simulation Settings: A uniform linear array with 10 elements, 3 radar targets at angles -40°/0°/40°, user count K=2/4/6, compared with the SDR and ZF schemes and the traditional SSP scheme that only reuses communication waveforms.
1  
The advantage of freedom is highlighted: when the number of users K is less than the required degree of freedom (4-dimensional in this paper), the SSP scheme suffers from severe beam distortion and fails to form a complete three-way main lobe; the proposed scheme can utilize all M=10-dimensional freedom, making the beam shape closer to that of a pure radar system.
2  
Performance trade-off rules: The higher the SINR threshold and the more communication users, the greater the radar beam error; under high communication quality requirements, the performance gap between ZF and SDR radars is significantly reduced.
3  
Radar perception test results: three indicators of target distance profile, angle estimation accuracy, and target detection probability. This paper's SDR scheme comprehensively outperforms the SSP; the advantage is most pronounced when K=2, with an RMSE in angle estimation close to that of a pure radar system.
4  
Communication Performance: The ZF scheme completely eliminates interference, resulting in higher user and rate; SDR only just meets the SINR lower limit, sacrificing some communication rate for better radar detection effect.
5  
Feasibility Boundary: When the SINR threshold is too high, both types of optimization models will encounter unsolvable situations. In engineering, it is necessary to set a reasonable communication minimum service threshold.
V. Summary of the Paper This paper designs a joint transmission beamforming architecture for MIMO radar and multi-user downlink communication in an integrated scenario, focusing on independent precoding of radar and communication waveforms. The core innovation and value are as follows:
1  
Architecture Innovation: Unlike traditional solutions that only reuse communication signals for radar detection, this approach separates the design of radar and communication precoding matrices. This allows MIMO radar to fully utilize all spatial degrees of freedom in the array, significantly improving multi-target beamforming, angle estimation, and target detection performance.
2  
Algorithmic Completeness: Two solution schemes proposed - semi-positive definite relaxation global optimal algorithm, theoretical proof of relaxation tightness, can obtain the optimal precoding. Low-complexity zero-forcing scheme significantly reduces computational overhead, and can replace the optimal algorithm in high-quality communication scenarios.
3  
Engineering adaptability: Using single-antenna equal power constraint, it fits the radar hardware emission characteristics. The system can be compatible with various scenarios such as user-target overlap / separation.
4  
Performance Advantage: Under the same communication SINR constraints, compared to traditional integrated solutions, radar performance loss is minimal. When setting communication thresholds reasonably, the detection capability approaches that of a purely radar device without communication coexistence.
Existing research still has limitations: there is a risk of unsolvability under extremely high SINR requirements. In the future, incorporating user communication rate benefits into the optimization objective function can replace the hard SINR lower limit constraints, broadening the system's application scope. This approach can be implemented in scenarios such as vehicle-to-infrastructure perception and communication integration, airborne radar communication co-aperture, and military frequency spectrum sharing, providing a new optimization approach for integrated transceiver beam design.

## Images

![Cooperative MIMO-ISAC System with Interference Rejection Beamforming [with MATLAB Code]](cover.jpg)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->
