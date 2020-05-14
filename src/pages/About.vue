<template>
  <Layout :show-logo="false">
    <!-- Author intro -->
    <Author :show-title="true" />

    <!-- List posts -->
    <div class="posts content">
      <PageTitle title="about" />
      <div class="post-card content-box">
        <p>Throughout my career I’ve often found myself working at the intersection of multiple disciplines within the digital ecosystem, including software engineering, analytics, product management and user experience. Combined with a keen interest in human behaviour and business processes, this has enabled me to build a diverse skill set and a thorough understanding of the ecosystem as a whole.</p>

        <p>
          I believe in
          <a
            href="https://github.com/tinavanschelt/lifelong-learning"
            target="_blank"
          >continuous learning</a>, embrace new challenges as growth opportunities and I always strive to surround myself with people who are smarter than me.
        </p>

        <p>Outside of work, I enjoy exploring, running, functional training, spending time outdoors and hiking with our dog, Wilco. I completed my first marathon in 2019 and am an avid member of the Parkrun community.</p>
        
        <h5>Timeline 👇🏼</h5>
        <p>
          For things I’m looking forward to and/or currently focused on visit
          <a href="/now">/now</a>
        </p>
      </div>
    </div>

    <div class="posts content">
      <div class="posts__title posts__title--year">2019</div>
      <div class="post-card content-box">
        <ul>
          <li>Ran my first marathon (Dublin 2019, 4:18:04), 50th Parkrun (March 2019) and fastest 21km, 10km and 5km</li>
          <li>Travelled through Ireland and hiked in the Wicklow mountains with Kevin (and Wilco)</li>
          <li>Travelled to Ontario (Toronto, Niagara Falls), New York State (Finger Lakes Distric, NYC), Northern Ireland, Wales, Copenhagen and Sweden (Malmo and Stockholm)</li>
          <li>Started studying at the University of Derby Online (BSc (Hons) Computing and Information Technologies)</li>
        </ul>
      </div>
    </div>

    <div class="posts content">
      <div class="posts__title posts__title--year">2018</div>
      <div class="post-card content-box">
        <ul>
          <li>Moved to Limerick, Ireland with Kevin (the husband) and Wilco (the dog)</li>
          <li>
            Made some new friends at
            <a href="http://www.weareleaf.com" target="_blank">Leaf</a> and started working remotely full-time
          </li>
          <li>Travelled to The Netherlands (Amsterdam, Zeeland), Brussels, Paris, Germany (Munich, Rothenburg, Heidelberg), Prague, Budapest and Canterbury - it was a good year for adventure.</li>
          <li>Learnt and applied Elixir, Phoenix and React-Native</li>
          <li>
            Completed 28
            <a
              href="http://www.parkrun.ie/results/athleteresultshistory/?athleteNumber=2367289"
              target="_blank"
            >parkruns</a> in 4 different countries
          </li>
        </ul>
      </div>
    </div>

    <div class="posts content">
      <div class="posts__title posts__title--year">2017</div>
      <div class="post-card content-box">
        <ul>
          <li>
            Joined the tech team at
            <a
              href="http://www.babylonstoren.com"
              target="_blank"
            >Babylonstoren</a>
          </li>
          <li>Primarily focused on Javascript (React) and frontend architecture</li>
          <li>Travelled to Greece &amp; Turkey</li>
          <li>Adopted Wilco (the dog)</li>
        </ul>
      </div>
    </div>

    <div class="posts content">
      <div class="posts__title posts__title--year">2016</div>
      <div class="post-card content-box">
        <ul>
          <li>
            Started at
            <a href="http://www.cybercellar.com" target="_blank">Cybercellar</a> as Ruby on Rails Frontend Developer
          </li>
          <li>Moved into our first house 🏡</li>
          <li>Travelled to Mozambique</li>
        </ul>
      </div>
    </div>

    <div class="posts content">
      <div class="posts__title posts__title--year">2015</div>
      <div class="post-card content-box">
        <ul>
          <li>Married a trooper 🍠</li>
          <li>Left Primedia Broadcasting to focus purely on my development skills</li>
          <li>Contract-based work (Frontend / Ruby on Rails)</li>
        </ul>
      </div>
    </div>

    <div class="posts content">
      <div class="posts__title posts__title--year">2014</div>
      <div class="post-card content-box">
        <ul>
          <li>Travelled to the UK (and attended the Next Radio conference)</li>
          <li>
            Agile Fundamentals Workshop,
            <a
              href="http://www.thoughtworks.com"
              target="_blank"
            >Thoughtworks</a>
          </li>
          <li>Still freelancing part-time as a web developer, picked up some international clients</li>
        </ul>
      </div>
    </div>

    <div class="posts content">
      <div class="posts__title posts__title--year">2013</div>
      <div class="post-card content-box">
        <ul>
          <li>Started working at Primedia Broadcasting as a Digital Product Owner</li>
          <li>
            Project Management &amp; UX Course, presented by
            <a
              href="http://www.twitter.com/RianVDM"
              target="_blank"
            >@RianVDM</a>
          </li>
          <li>
            <a href="http://thebookmarks.co.za/" target="_blank">The Bookmarks</a> 2013 Shortlist, Best Student
          </li>
        </ul>
      </div>
    </div>

    <div class="posts content">
      <div class="posts__title posts__title--year">2012</div>
      <div class="post-card content-box">
        <ul>
          <li>Graduated from Red &amp; Yellow</li>
          <li>
            Completed my first
            <a
              href="http://www.twooceansmarathon.org.za/"
              target="_blank"
            >Two Oceans Half Marathon</a>
          </li>
          <li>
            Student Finalist at the
            <a href="http://www.loeries.com/" target="_blank">Loerie Awards</a>
          </li>
        </ul>
      </div>
    </div>

    <div class="posts content">
      <div class="posts__title posts__title--year">2011</div>
      <div class="post-card content-box">
        <ul>
          <li>Mostly a combination of studying and freelancing</li>
          <li>Completed the Certificate Wine Course at the Cape Wine Academy</li>
          <li>
            Worked as a Digital Intern at
            <a
              href="http://www.saatchi.co.za/"
              target="_blank"
            >Saatchi &amp; Saatchi</a>
          </li>
        </ul>
      </div>
    </div>

    <div class="posts content">
      <div class="posts__title posts__title--year">2010</div>
      <div class="post-card content-box">
        <ul>
          <li>
            Started studying Design &amp; Art Direction at
            <a
              href="https://www.redandyellow.co.za/"
              target="_blank"
            >Red &amp; Yellow</a>
          </li>
          <li>Started freelancing as a web developer</li>
          <li>
            Completed the South African Wine Course at the
            <a
              href="http://www.capewineacademy.co.za"
              target="_blank"
            >Cape Wine Academy</a>
          </li>
        </ul>
      </div>
    </div>

    <div class="posts content">
      <div class="posts__title posts__title--year">2009</div>
      <div class="post-card content-box">
        <ul>
          <li>Took a gap year</li>
          <li>
            Completed a Comprehensive Web Design course at
            <a
              href="https://www.cityvarsity.co.za/"
              target="_blank"
            >City Varsity</a>
          </li>
          <li>Worked, travelled (and bought my first MacBook Pro 👩🏼‍💻) in the USA</li>
          <li>Started working as a Junior Web Designer at a small agency in Cape Town</li>
        </ul>
      </div>
    </div>
  </Layout>
</template>

<page-query>
query {
  posts: allPost(filter: { published: { eq: true }}) {
    edges {
      node {
        id
        title
        date (format: "D MMMM YYYY")
        timeToRead
        description
        path
        tags {
          id
          title
          path
        }
      }
    }
  }
}
</page-query>

<script>
import Author from "~/components/Author.vue";
import PageTitle from "~/components/PageTitle.vue";
import PostCard from "~/components/PostCard.vue";

export default {
  components: {
    Author,
    PageTitle,
    PostCard
  },
  metaInfo: {
    title: "About"
  }
};
</script>

<style lang="scss">
.posts {
  &__title {
    font-size: 6rem;
    font-weight: 600;
    margin-bottom: -3.35rem;
    opacity: 0.1;
    padding: 0 2rem;

    &--year {
      font-size: 3.5rem;
      margin-bottom: -2rem;
    }
  }
}
</style>





