# Title

I have a JSON file with data for reviews.
And I also have this component, which recieves the data from the json file:

```js
---
export interface Props {
  name: string;
  date: string;
  text: string;
}

const { name, date, text } = Astro.props;
---

<div class="slide-mobile" id="slide1">
  <div class="slide-text">
    <div>
      <p>
        {text}
      </p>
      <p>- {name}, {date}</p>
    </div>
  </div>
</div>

<style>
  .slide-mobile {
    width: 100%;
    height: 100%;
    max-width: 1020px;
    position: absolute;
    transition: all 0.5s;
    display: flex;
    gap: 16px;
    padding-right: 1px;
  }

  .slide-text {
    width: 100%;
    display: flex;
    align-items: center;
    background-color: rgba(255, 255, 255, 0.674);
    padding: 20px;
    border-radius: 4px;
  }

  .slide-text :first-child,
  .slide-text-vertical-center :first-child {
    padding-bottom: 10px;
  }
</style>

```

Below is the code which uses the component and maps through the array with reviews:

```js
---
import { Icon } from "astro-icon";
import Review from "./Review.astro";
import json from "../data/reviews.json";

const data = json;
---

<!-- Slider container -->
<div class="slider-mobile">
  <!-- slide 1 -->
  {
    data.map((review) => (
      <Review name={review.name} date={review.date} text={review.text} />
    ))
  }

  <!-- Control buttons -->
  <button class="btn-m btn-next-m">
    <Icon name="fa-solid:chevron-right" class="btn-icon" />
  </button>
  <button class="btn-m btn-prev-m">
    <Icon name="fa-solid:chevron-left" class="btn-icon" />
  </button>
</div>

<script>
  // Select all slides
  const slides = document.querySelectorAll(".slide-mobile");

  // loop through slides and set each slides translateX
  slides.forEach((slide, indx) => {
    slide.style.transform = `translateX(${indx * 100}%)`;
  });

  // select next slide button
  const nextSlide = document.querySelector(".btn-next-m");

  // current slide counter
  let curSlide = 0;
  // maximum number of slides
  let maxSlide = slides.length - 1;

  // add event listener and navigation functionality
  nextSlide.addEventListener("click", function () {
    // check if current slide is the last and reset current slide
    if (curSlide === maxSlide) {
      curSlide = 0;
    } else {
      curSlide++;
    }

    //   move slide by -100%
    slides.forEach((slide, indx) => {
      slide.style.transform = `translateX(${100 * (indx - curSlide)}%)`;
    });
  });

  // select prev slide button
  const prevSlide = document.querySelector(".btn-prev-m");

  // add event listener and navigation functionality
  prevSlide.addEventListener("click", function () {
    // check if current slide is the first and reset current slide to last
    if (curSlide === 0) {
      curSlide = maxSlide;
    } else {
      curSlide--;
    }

    //   move slide by 100%
    slides.forEach((slide, indx) => {
      slide.style.transform = `translateX(${100 * (indx - curSlide)}%)`;
    });
  });
</script>

<style>
  .slider-mobile {
    width: 100%;
    /* max-width: 800px; */
    min-height: 370px;
    position: relative;
    overflow: hidden;
  }

  .slider-mobile img {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }

  .btn-m {
    position: absolute;
    width: 40px;
    height: 40px;
    padding: 10px;
    border: none;
    border-radius: 50%;
    z-index: 10px;
    cursor: pointer;
    background-color: #feeafa;
    opacity: 0.8;
    font-size: 18px;
  }

  .btn-m:active {
    transform: scale(1.1);
  }

  .btn-icon {
    height: 20px;
    color: #8e9aaf;
  }

  .btn-prev-m {
    top: 45%;
    left: 2%;
  }

  .btn-next-m {
    top: 45%;
    right: 2%;
  }

  @media (max-width: 960px) {
  }

  @media (max-width: 600px) {
    .slide-mobile {
    }
  }
</style>



```

I need to change the code below so I could map through the array of reviews and show 3 revies in one slide:

```js
---
import { Icon } from "astro-icon";
import Review from "./Review.astro";
import json from "../data/reviews.json";

const dataDesktop = json;
---

<!-- Slider container -->
<div class="slider-desktop">
  <!-- slide 1 -->

  <!-- {
    dataDesktop.map((review) => (
      <Review name={review.name} date={review.date} text={review.text} />
    ))
  } -->

  <div class="slide">
    <div class="slide-text">
      <div>
        <p>
          Zu Elena gehe ich immer sehr gerne in die Massage. Die Session ist
          professionell, einfühlsam und Elena ist auch immer für einen Quatsch
          zu haben, und findet immer die richtige Mischung von Stille, Massage
          und Gespräch. Ich schätze Elena sehr und freue mich jeweils auf die
          Massage.
        </p>
        <p>- Flavio, 10. Mai 2023</p>
      </div>
    </div>
    <div class="slide-text">
      <div>
        <p>
          Hallo liebe Elena. Ich gehe jedesmal völlig entspannt bei dir aus der
          Massage. Sobald ich eine Verspannung oder ähnliche Probleme habe gehst
          du darauf ein und versuchst es zu lösen. Der Wohlfühlfaktor ist bei
          dir einfach gegeben und du weisst was du tust. 👌🏻 Ich komme immer
          wieder gern zu dir mach weiter so. Liebe Grüsse Claudia
        </p>
        <p>- Claudia, 11. Mai 2023</p>
      </div>
    </div>
    <div class="slide-text">
      <div>
        <p>
          Absolut entspannende, verspannungslösende Massage bei Elena! Sie
          trifft die richtigen Punkte mit dem richtigen Druck. Komme immer
          wieder gerne.
        </p>
        <p>- Sylvie, 11. Mai 2023</p>
      </div>
    </div>
    <!-- <img src={slideImg1} alt="" /> -->
  </div>

  <!-- slide 2 -->
  <div class="slide">
    <div class="slide-text">
      <div>
        <p>
          Jedes Mal eine tolle, entspannende und einfühlsame Massage.
          Verspannungen werden gelöst und ich gehe mit einem gutem Gefühl.
          Nebenbei findet man auch immer Themen zum quatschen. Sehr zu
          empfehlen, Elena weiss was sie tut. Danke.
        </p>
        <p>- Marco, 12. Mai 2023</p>
      </div>
    </div>
    <div class="slide-text">
      <div>
        <p>
          Meine kleine Auszeit bei Elena geniesse ich immer sehr! Sie hat mir
          durch ihre zielgerichtete Massage schon oft den Gang zum
          Chiropraktiker erspart, dafür bin ich sehr dankbar.
        </p>
        <p>- Imke, 25. Mai 2023</p>
      </div>
    </div>
    <div class="slide-text">
      <div>
        <p>
          Liebe Elena. Deine (fast) wöchentliche Massagen sind meine Rettung für
          meinen verspannten Oberkörper. Vorallem meine Schultern sind vom Sport
          so verkrampft. Nach deiner Massage fühle ich mich tausend mal
          entspannter und seitdem ich mich bei dir massieren lasse, habe ich
          weniger Schmerzen und kann so noch mehr Sport machen :D Letztens hast
          du eine Verklemmung im Nacken-Schulterbereich weggezaubert! Vielen
          Vielen Dank für deine magischen Hände und dass du immer auf meine
          Wehwehchen schaust :)
        </p>
        <p>- Nguyen, 1. Septmber 2023</p>
      </div>
    </div>
  </div>

  <!-- slide 3 -->
  <div class="slide">
    <div class="slide-text">
      <div>
        <p>
          Ohne die Massagen bei Elena kann ich es mir nicht mehr vorstellen: mit
          ihrer grosser Erfahrung löst sie immer meine Verspannungen und trägt
          dank einer angenehmen Atmosphäre sehr zu meiner Entspannung bei.
        </p>
        <p>- Sara, 24. Oktober 2023</p>
      </div>
    </div>
    <div class="slide-text">
      <div>
        <p>
          Seit langer Zeit lasse ich mich wöchentlich von meinen Verspannungen
          erlösen, die sich aufgrund meiner PC-Arbeit ergeben. Elena geht auf
          meine Wünsche ein und kümmert sich auch um meinen Muskelkater, wenn
          ich zu lange mit dem Joggen aufgehört habe. Ich vermisse die
          Behandlungen, wenn sie wegen Ferien oder Krankheit ausfallen. Elena,
          vielen Dank für Deine Massagen und auch die Gespräche. Bis im Neuen
          Jahr, ich freue mich
        </p>
        <p>- Daniela, 21. December 2023</p>
      </div>
    </div>
    <div class="slide-text">
      <div>
        <p>
          Ich bin dankbar für Elenas Hände, ihren Spirit und Können 🌺 man/frau
          kriecht rein und fliegt neu geboren raus 🌺 Danke Elena, wunderbar
          kann ich mich so nah unter deine Hände geben 🏆
        </p>
        <p>- Nicole, 24. Januar 2024</p>
      </div>
    </div>
  </div>

  <!-- slide 4 -->
  <div class="slide">
    <div class="slide-text">
      <div>
        <p>
          Als Bildschirmarbeiter habe ich sehr häufig mit einer verspannten
          Nacken- und Schulterpartie zu kämpfen. Elena kann diese Verspannungen
          lösen und hilft mir dabei sehr, in den Tagen nach der Massage
          schmerzfrei zu sein. Dank Ihrer Erfahrung und den vielen Techniken,
          die Elena beherrscht, kommt sie gut und einfühlsam an die Quellen der
          Verspannungen und kann sie lösen. Ich bin sehr dankbar für Ihre Hilfe.
        </p>
        <p>- Dirk, 14. März 2024</p>
      </div>
    </div>
    <div class="slide-text">
      <div>
        <p>
          Liebe Elena, ich freue mich auf jeden Termin bei Dir. Die Entspannung
          beginnt mit der fröhlichen Begrüssung. Die Zeit in der Massage ist
          eine willkommene Oase im Arbeitsalltag, die Massage sehr
          professionell, abwechslungsreich und entspannend. Immer gerne wieder!
        </p>
        <p>- Martin, 25. März 2024</p>
      </div>
    </div>
    <div class="slide-text">
      <div>
        <p>
          Die Massagen sind super bei Elena. Sie geht individuell auf die
          Bedürfnisse ein, löst die Verspannungen und schafft jedes mal eine
          entspannte Atmosphäre. Ich fühle mich jedes mal wie neugeboren und
          kann Elena wärmstens weiterempfehlen.
        </p>
        <p>- Barbara, 27. März 2024</p>
      </div>
    </div>
  </div>

  <!-- slide 5 -->
  <div class="slide">
    <div class="slide-text">
      <div>
        <p>
          Elena ist sehr kompetent, engagiert und empathisch - vor der Massage
          erfolgt eine kurze "Aufnahme" der körperlichen Situation - nach der
          Massage gibt sie wertvolle Tipps zur Selbst-Hilfe - während der
          Massage ist sie im guten Kontakt und findet so die richtige
          Intensität. Sehr zu empfehlen.
        </p>
        <p>- Gernot, 19. Juli 2024</p>
      </div>
    </div>
    <div class="slide-text">
      <div>
        <p>
          Hat mir gut gefallen. Elena hat zugehört, was ich brauche und die
          Massage entsprechend ausgerichtet. Sehr entspannend. Danke!
        </p>
        <p>- Kiki, 19. April 2024</p>
      </div>
    </div>
    <div class="slide-text">
      <div>
        <p>Sehr professionell. Ich kann es nur empfehlen. Danke</p>
        <p>- Magdalena, 19. April 2024</p>
      </div>
    </div>
  </div>

  <!-- slide 6 -->
  <div class="slide">
    <div class="slide-text">
      <div>
        <p>Danke Elena für die sehr wohltuende und entspannende Massage!</p>
        <p>- Lukasz, 15. Dezember 2023</p>
      </div>
    </div>
    <div class="slide-text">
      <div>
        <p>
          Sehr empfehlenswert. Elena hat viel Erfahrung und eine Behandlung bei
          ihr tut einem einfach rundum gut.
        </p>
        <p>- Teresa, 18. Dezember 2023</p>
      </div>
    </div>
    <div class="slide-text">
      <div>
        <div>
          <p>Sehr angenehm. Elena ist sehr kompetent und freundlich.</p>
          <p>- Melanie, 1. Februar 2024</p>
        </div>
        <br />
      </div>
    </div>
  </div>

  <!-- slide 7 -->
  <div class="slide">
    <div class="slide-text">
      <div>
        <p>
          Sehr angenehm und hat gut geholfen, gegen meine sehr starken
          Verspannungen.
        </p>
        <p>- Nuria, 12. Januar 2024</p>
      </div>
    </div>
    <div class="slide-text">
      <div>
        <p>
          Super freundlich, sehr kompetent und engagiert! Werde nun regelmässig
          zu ihr gehen!!
        </p>
        <p>- Raphael, 29. März 2024</p>
      </div>
    </div>
    <div class="slide-text">
      <div>
        <p>
          Die Behandlung war top und ich habe gute Tipps für den Alltag erhalten
          - vielen Dank!
        </p>
        <p>- Regula, 5. Januar 2024</p>
      </div>
    </div>
  </div>

  <!-- slide 8 -->
  <div class="slide">
    <div class="slide-text">
      <div>
        <p>Super entspannend !</p>
        <p>- Andreas, 2. Februar 2024</p>
      </div>
    </div>
    <div class="slide-text">
      <div>
        <p>Wunderbar entspannend! Danke!</p>
        <p>- Sylvia, 16. Februar 2024</p>
      </div>
    </div>

    <div class="slide-text slide-text-empty">
      <p>Hier könnte Ihre Bewertung sein</p>
    </div>
  </div>

  <!-- Control buttons -->
  <button class="btn btn-next">
    <Icon name="fa-solid:chevron-right" class="btn-icon" />
  </button>
  <button class="btn btn-prev">
    <Icon name="fa-solid:chevron-left" class="btn-icon" />
  </button>
</div>

<script defer>
  // Select all slides
  const slides = document.querySelectorAll(".slide");

  // loop through slides and set each slides translateX
  slides.forEach((slide, indx) => {
    slide.style.transform = `translateX(${indx * 100}%)`;
  });

  // select next slide button
  const nextSlide = document.querySelector(".btn-next");

  // current slide counter
  let curSlide = 0;
  // maximum number of slides
  let maxSlide = slides.length - 1;

  // add event listener and navigation functionality
  nextSlide.addEventListener("click", function () {
    // check if current slide is the last and reset current slide
    if (curSlide === maxSlide) {
      curSlide = 0;
    } else {
      curSlide++;
    }

    //   move slide by -100%
    slides.forEach((slide, indx) => {
      slide.style.transform = `translateX(${100 * (indx - curSlide)}%)`;
    });
  });

  // select prev slide button
  const prevSlide = document.querySelector(".btn-prev");

  // add event listener and navigation functionality
  prevSlide.addEventListener("click", function () {
    // check if current slide is the first and reset current slide to last
    if (curSlide === 0) {
      curSlide = maxSlide;
    } else {
      curSlide--;
    }

    //   move slide by 100%
    slides.forEach((slide, indx) => {
      slide.style.transform = `translateX(${100 * (indx - curSlide)}%)`;
    });
  });
</script>

<style>
  .slider-desktop {
    width: 100%;
    /* max-width: 800px; */
    min-height: 370px;
    position: relative;
    overflow: hidden;
  }

  .slide {
    width: 100%;
    height: 100%;
    max-width: 1020px;
    position: absolute;
    transition: all 0.5s;
    display: flex;
    gap: 16px;
    padding-right: 1px;
  }

  .slide-text {
    width: 100%;
    display: flex;
    align-items: center;
    background-color: rgba(255, 255, 255, 0.674);
    padding: 20px;
    border-radius: 4px;
  }

  .slide-text :first-child {
    padding-bottom: 10px;
  }

  .slide-text-empty {
    width: 100%;
    background-color: rgba(255, 255, 255, 0.674);
    padding: 20px;
    border-radius: 4px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .slide img {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }

  .btn {
    position: absolute;
    width: 40px;
    height: 40px;
    padding: 10px;
    border: none;
    border-radius: 50%;
    z-index: 10px;
    cursor: pointer;
    background-color: #feeafa;
    opacity: 0.8;
    font-size: 18px;
  }

  .btn:active {
    transform: scale(1.1);
  }

  .btn-icon {
    height: 20px;
    color: #8e9aaf;
  }

  .btn-prev {
    top: 45%;
    left: 2%;
  }

  .btn-next {
    top: 45%;
    right: 2%;
  }

  .no-review {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  @media (max-width: 960px) {
  }

  @media (max-width: 600px) {
  }
</style>


```
