## Introduction
I played an instrumental role in developing Stout's Mobile Augmented Reality Tour of Art (SMART-Art), currently used by UW-Stout's digital humanities department to provide context and insight into the numerous murals around campus. This is a framework that has been used by dozens of students to augment murals and canvas pieces with future plans to expand to 3d sculptures once more funding is available.

I was interviewed by the school paper [here](https://www.uwstout.edu/about-us/news-center/students-create-augmented-reality-app-provide-insight-campus-artworks), but allow me to go into more detail below.

I was one of three hires for this project and over time I ended up representing and leading this team. The team consisted of myself, [Brian Bauch](https://www.linkedin.com/in/brian-bauch-5a656493/), and [Harrison Kelly](https://www.linkedin.com/in/harrison-kelly-14b925197/). Brain and Harrison, like me, were also students of game design at Stout, computer science, and art respectively. Prof. Mitch Ogden, associate professor, and program director, originally came up with this idea and pitched it to [The Menard Center for the Study of Institutions and Innovation](https://www.uwstout.edu/csii) (MCSII) which was our patron. Prof. Ogden, having experimented in the past with Adobe's AR products with little success, gave us free rein to choose the technology/stack that would be best given his requirements. This was a tremendous responsibility that placed us at the very beginning of the software development lifecycle.

## Specifics
Specifically, I was responsible for the creation of a pipeline that would be used by students to create the AR experience. Given the student's knowledge of unity would be minimal to none, I had to create a pipeline that would limit the amount of work done in unity as much as possible. Thanks to [XuidUnity](https://github.com/itouh2-i0plus/XuidUnity) an open-source and MIT-licensed plugin, the pipeline would start in Adobe XD where the students would create a template with their content. Then it would be exported from Adobe XD and imported into unity where the template would be converted into Unity UI objects. I then built in-engine tools ranging from instantiating necessary objects in the unity hierarchy to automatically finding and connecting objects in the inspector. It was important to get the pipeline as simple as possible because I knew I would be instructing students and future developers on how to use it in the future.

![tour](gifs\tour.gif)

*Tour of our office's whiteboards. The first board is relevant, it visualizes the pipeline and outlines dev vs student responsibility.*

In addition, I was responsible for creating the 'stack' concept and implementing it through C# scripts. A card stack is an array of 'cards' which can be interacted with using touch controls. These cards hold the content created by the student and act almost like a PowerPoint presentation. The cards can contain text, images, video, and sound. A card stack is a child of a 'point of interest', which is placed on a spot of the painting that the students want to draw attention to, be they a character's hat, the artist's signature, or the frame the painting is in.



![whiteboard interactions](images\whiteboard-interaction.jpg)
*Planning interactions on the whiteboard*

![whiteboard debugging](images\working-through-my-own-loop.jpg)
*Debugging card initialization by stepping through the loop logic on the whiteboard*

![interaction prototype](gifs\interaction-prototype.gif)

*Gif of a very early interaction prototype.*

This project presented a significant challenge that required expertise in objects, loops, arrays, and linear interpolation, as well as patience to revise and debug until it met our expectations. Below is a code snippet of how I moved the cards using linear interpolation. The GitHub for [SMART-Art](https://github.com/karrjm/SMARTArt) is public if you would like to check it out. I am happy to go into detail or show more examples if there is interest.

```C#
private void MoveCards()
{
  // This loop moves the cards.
  for (var i = 0; i < cards.Length; i++)
  {
    // Use linear interpolation to 'swipe' the cards left and right
    cards[i].localPosition = Vector3.Lerp(cards[i].localPosition, _cardPositions[i + stackScale + _cardArrayOffset], Time.deltaTime * cardMoveSpeed);

    // Slot the card in position when it gets close enough.
    if (Mathf.Abs(cards[i].localPosition.x - _cardPositions[i + stackScale + _cardArrayOffset].x) < 0.01f)
    {
      cards[i].localPosition = _cardPositions[i + stackScale + _cardArrayOffset];

      var cg = cards[i].gameObject.GetComponent<CanvasGroup>();

      // Disables interaction with cards that are not on top of the stack and calls the UIFader.
      if (cards[i].localPosition.x == 0)
      {
        cg.interactable = true;
        cards[i].SetAsLastSibling();
        _fader.FadeIn(cg);
      }
      else
      {
        cg.interactable = false;
        _fader.FadeOut(cg);
      }
    }
  }
}
```

I was later asked to be a teacher's assistant in Prof. Ogden's upcoming course which would use SMART-Art. I spent most of my time outside of class fixing any bugs that emerged with real-world use. More importantly during class, I found a new appreciation for teaching people to use something I had designed, answering questions about it in a non-technical and everyday language, and spending time with students troubleshooting or lending my expertise to help them create something they are proud of. 

Overall, my team and I made a big impact by delivering what we did. Prof. Ogden and MCSII were impressed with our work and SMART-Art is still being by the digital humanities department to create augmented reality experiences. Mitch Ogden has graciously offered himself as a reference which I am happy to provide if you folks at **Hack Club** request.
