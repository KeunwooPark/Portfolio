---
layout: post

title: "FoolProofJoint: Reducing Assembly Errors of Laser Cut 3D Models by Means of Custom Joint Patterns"
permalink: /foolproofjoint/
thumbnail: /assets/img/foolproofjoint/thumbnail.png
publish_date: 2022-04-29
venue: CHI 2022
authors: "**Keunwoo Park**, Conrad Lempert , Muhammad Abdullah, Shohei Katakura, Jotaro Shigeyama, Thijs Roumen, Patrick Baudisch"
to_appear: no
award:
video_embed_link : https://www.youtube.com/embed/YlfSQhLRqSc
tags:
categories: conference
---
# Abstract
![fig1](/assets/img/foolproofjoint/foolproofjoint-01.png)
We present FoolProofJoint, a software tool that simplifies the assembly of laser-cut 3D models and reduces the risk of erroneous assembly. FoolProofJoint achieves this by modifying finger joint patterns. Wherever possible, FoolProofJoint makes similar looking pieces fully interchangeable, thereby speeding up the user's visual search for a matching piece. When that is not possible, FoolProofJoint gives finger joints a unique pattern of individual finger placements so as to fit only with the correct piece, thereby preventing erroneous assembly. In our benchmark set of 217 laser-cut 3D models downloaded from kyub.com, FoolProofJoint made groups of similar looking pieces fully interchangeable for 65% of all groups of similar pieces; FoolProofJoint fully prevented assembly mistakes for 97% of all models.

# Problems and FoolProofJoint's Solutions
When users assemble a 3D laser-cut model, they fnd themselves looking at a 2D layout of pieces, sometimes referred to as a cutting plan, as illustrated by (b) of the right below figure. Users will typically pick up a piece and then look for the matching piece to attach. They may now limit their search to pieces that feature an edge of matching length; In addition, they may have expectations about the overall shape of the desired piece that help them select which piece to pick up. The assembly situation may thus present itself to the user roughly as illustrated by (c) of the right below figure, where the model’s 23 pieces fall into a smaller number of groups based on their rough shape (in the paper, we refered to this shape, i.e., the piece with the fnger joints flled in, as envelope). Parts within each group may still difer by their joints—however, users tend to have less clear expectations about what shape of joint they are looking for, as they may fnd it hard to interpret joint patterns quickly enough to consider them in their visual search. In this situation, two types of assembly errors tend to emerge. First, users may reach for an incorrect piece (even if it is from that piece group). Second, for pieces with a symmetric envelope, users may try to assemble a piece in the wrong orientation. In the following, we go over these two classes of assembly errors and how FoolProofJoint addresses them.

![fig2](/assets/img/foolproofjoint/foolproofjoint-02.png)

## Issue 1: Users pick up similar looking pieces

The first figure takes a closer look at what happens during assembly. Here the user is trying to assemble one of the chair’s legs. The user is looking for a piece that could fit, thus considers all pieces that feature roughly the dimensions of a leg. Ideally, the user picks the correct piece and successfully assembles it. However, this may not be the most likely outcome, given that there are eight pieces of the same envelope and two more pieces with a similar envelope. There are three other possible outcomes with increasingly dire consequences.

1. The user picks up an incorrect piece, tries to assemble it, and instantaneously recognizes that it does not fit because their joints collide and cannot be assembled. This case causes little damage, as the user simply drops the piece and tries a diferent one.  
2. Less ideally, the incorrect piece still fits. The user attaches it to then now realize that it is incorrect, e.g., because adjacent joints are not aligned. The user removes the piece again and tries a diferent one. Removing a piece, however, produces a lasting negative efect on both pieces involved, as it wears out their joints, making the fnal model less sturdy.
3. The worst possible outcome is illustrated in the right below figure: (a) an erroneous piece may fit, and the error may go unnoticed, as subsequent pieces continue to fit as well, thereby producing a propagation error. (b) By the time the user recognizes the issue, the user has to dis- and re-assemble multiple pieces.

![fig3](/assets/img/foolproofjoint/foolproofjoint-03.png)

### FoolProofJoint makes joints diferent. 

FoolProofJoint addresses these assembly mistakes by giving each joint pair a unique fnger pattern, which assures that only the correct piece can be assembled. This prevents erroneous assembly and thus also propagation errors by making users aware of their error before any piece can be assembled, giving users the opportunity to put the piece back and try again. The below figure shows an example. (a) The user should assemble the front leg piece to the seat bottom piece, but accidentally assembles the backrest piece, which just happens to have a similar envelope. (b) FoolProofJoint resolves this by making the two joint patterns distinct, preventing the incorrect piece from being assembled. 

![fig4](/assets/img/foolproofjoint/foolproofjoint-04.png)

### FoolProofJoint makes pieces interchangeable, by making their joints the same. 

The issue discussed above is caused by pieces initially looking the same to the user because they are similar, but then turning out diferent later when the user notices their diferences led to assembly mistakes. However, if pieces were the same all the way, no problem would ever arise. FoolProofJoint exploits this idea. Before making any joints diferent, FoolProofJoint tests whether it might be possible to make pieces fully identical and thus interchangeable. FoolProofJoint achieves this by checking whether the envelopes are the same, whether any potentially present features such as cutouts and engravings are the same, and whether the model geometry ofers sufcient degrees of freedom to adjust all involved joints (see Section 5). If that is the case, FoolProofJoint make the joints of all pieces which mutually meet these criteria (called a piece group) identical. The below figure shows an example. Here FoolProofJoint makes the joints of all eight leg pieces of the chair model identical, thus the pieces are fully interchangeable. This not only eliminates the risk of assembly errors, but also vastly reduces the visual search to find correct pieces. 

![fig5](/assets/img/foolproofjoint/foolproofjoint-05.png)

## Issue 2: Users accidentally re-orient symmetric pieces

The second issue that leads to assembly errors are symmetric piece envelopes. Looking back at the figure of the chair pieces shows that all pieces except for the ones in group 11 feature some sort of symmetry; several pieces even feature multiple symmetries. As a result, users might try to assemble e.g., the chair’s feet in any of the four possible orientations. As a result, we obtain the same range of outcomes, i.e., the user may attach the piece in the correct orientation or—the user may try an incorrect orientation, causing the piece either not to ft, or to ft despite the incorrect orientation as illustrated by the right below figure, which leads to propagation errors. In our experience, issues resulting from symmetrical piece envelopes even happen when users are provided with an instruction manual: Assembly instructions will often do a good job at guiding users to the right piece and even the right orientation. However, once users have picked up that piece, the piece’s original orientation is quickly lost, as the piece gets rotated or flipped in the user’s hand. 

![fig6](/assets/img/foolproofjoint/foolproofjoint-06.png)

### FoolProofJoint makes symmetric joints. 

FoolProofJoint addresses issues resulting from symmetric piece envelopes in a similar way to how it addresses confusion of pieces. FoolProofJoint starts by testing whether it might be possible to make the joints of the piece symmetric in a way that matches its symmetries. FoolProofJoint achieves this by checking whether the piece is symmetric, whether no features such as cutouts or engravings are present that might break symmetries, and whether the model geometry ofers sufcient degrees of freedom to adjust all involved joints (see `Section 5: Algorithm' of the paper). If that is the case, FoolProofJoint makes the piece’s joints identical according to the piece’s symmetry (the right below figure). 

![fig7](/assets/img/foolproofjoint/foolproofjoint-07.png)

# Contributions
In this paper, we make four contributions. First, we categorize and describe common classes of mistakes users make when assembling laser-cut 3D models, namely those stemming from similar and symmetric pieces. Second, we demonstrate how to resolve these issues by making pieces interchangeable when possible and distinct otherwise. Third, we present an algorithm that maximizes the efect of our approach by means of global optimization. Finally, we validate our approach by means of a technical evaluation involving 217 laser-cut 3D models, where FoolProofJoint made groups of similar looking pieces fully interchangeable for 65% of all groups of similar pieces and fully prevented assembly mistakes for 97% of all models. The beneft of our approach is faster, more reliable assembly. Limitations include that our current implementation is limited to finger joints. 

**For more details, please see our paper.**

# Resources
- [paper](https://dl.acm.org/doi/abs/10.1145/3491102.3501919)
- [longer video](https://youtu.be/HP_hydMD6NE)
- [talk](https://youtu.be/s-hUrZeo6sM)
