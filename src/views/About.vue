<template>
  <header id="site-header" class="container">
    <article class="section">
        <router-link to="/"><img src="./../assets/logo.svg"></router-link>
    </article>
  </header>
  <section id="balance" class="ll container strip">
      <article class="section banner">
        <p>
            <router-link to="/">&larr; Back</router-link>
        </p>
        <h1>About</h1>
      </article>
  </section>
  <main class="home ll container">
    <article class="section">
      <p>
        Landlord List is a site put together by a few volunteers with the aim of highlighting the issue of
        landlords' interests having an increasing impact on our society.
      </p>
      <p>
        Everywhere you look: town centres are being transformed as developers convert shops and old clubs into flats,
        more and more people are renting with no hope of ever being able to own, and planning laws are being changed to
        make it easier for developers to turn inapropriate buildings into flats.
      </p>
      <p>
        The real issue is many people of the people in power in this country are landlords, and so their interests
        are aligned with the owners, not the majority of people who are tennants or own the home they live in.
      </p>
      <h2>FAQs</h2>
      <h3>Where does the information come from?</h3>
      <p>
        Parliament publishes a register of interests for Lords and MPs. This includes details of any property they own
        valued over £100,000 or (importantly for us) any property for which they recieve rental income over £10,000
        per year.
      </p>
      <p>
        We use parliament's software (and our eyeballs) to collect this information and present it to you in an easy to digest
        way.
      </p>
      <h3>Are you going to add local councillors?</h3>
      <p>Maybe... but probably not. There's no standard way this information is available so it would be a huge project to
        gather the data from every local council and collate it into a database.</p>
      <p>Even if we did that, local elections are held every year - so we'd need to repeat the process every 12 months.</p>

      <h3>Will you add Scottish and Welsh MPs?</h3>
      <p>We do aim to add information on Scottish MPs, members of the Welsh Assembly, and members of the Northern Irish Assembly 
        in due course. The information is available but requires us to analyse it and add it to this site.</p>
      <p>We'll also need to update this site to handle those bodies so, it won't be soon!</p>

      <p>
        <router-link to="/">&larr; Back</router-link>
      </p>
    </article>
  </main>
</template>

<style lang="scss" scoped>
#site-header {

  img {
    max-height: 80px;
    width: min-content;
    margin: .44rem 0;
  }

  > .section {
    display: flex;
    justify-content: center;
    align-items: center;
  }

}

.banner {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding-top: .88rem;
}

.strip {
    background-color: #fff;
    border-bottom: 1px solid darken(#fff, 10%)
}

</style>