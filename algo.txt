window.addEventListener("load", function () {
  let reviewIdList = [
    "6348cc88377e522e314e8cd9",
    "6051c2f2f85d7503942cc4f1",
    "60a1e5e1f9f4870b7005802b",
    "6058c385f85d7507e41d8465",
    "625427ebc7628b203ba92ce3",
    "6348ad8f377e522e314e86c2",
    "63c57e994d0773066a39d1a2",
    "63482c8e377e522e314e215c"
  ];
  function fetchTrustPilotData() {
    for (let reviewID of reviewIdList) {
      fetch(
        "https://api.trustpilot.com/v1/reviews/" +
          reviewID +
          "?apikey=GAHGm02p2uPisGA2X2E88ypHqQnDmxwK"
      )
        .then((response) => {
          if (!response.ok) {
            throw Error("ERROR");
          }
          return response.json();
        })
        .then((data) => {
          console.log(data);
          let consumer_profile_img_link_73x73;
          fetch(
            "https://api.trustpilot.com/v1/consumers/" +
              data.consumer.id +
              "/profile?apikey=GAHGm02p2uPisGA2X2E88ypHqQnDmxwK"
          )
            .then((response) => {
              if (!response.ok) {
                throw Error("ERROR");
              }
              return response.json();
            })
            .then((consumer_profile_data) => {
              // console.log(data);
              if (consumer_profile_data.hasImage) {
                consumer_profile_img_link_73x73 =
                  consumer_profile_data.profileImage.image73x73.url;
              }
              let cardHTML = `<div class="customer-review-item swiper-slide">
            <div class="customer-review-content">
              <h3 class="heading-m black tablet-text-28 mobile-text-24 s-m-b-20">${data.title}</h3>
              <p class="p-jumbo black s-m-b-20">${data.text}</p>
              <div class="customer-review-5-star-img" alt="Trustpilot review star" ></div>
              <div class="customer-review-profile">
                <img src="${consumer_profile_img_link_73x73}" alt="" class="review-discord-icon">
                <p class="heading-s black">${data.consumer.displayName}</p>
              </div>
            </div>
        </div>`;
              $("#cards-list-api-ver").prepend(cardHTML);
            })
            .catch((error) => {
              console.log(error);
            });

          // <div class=""></div>
        })
        .catch((error) => {
          console.log(error);
        });
    }
  }
  fetchTrustPilotData();
  var flkty;
  let checkingSliderInterval = setInterval(() => {
    let totalReviews = reviewIdList.length;
    let totalReviewLoaded = $(".customer-review-item.swiper-slide:not(.hidden)")
      .length;
    if (totalReviews === totalReviewLoaded - 1) {
      clearInterval(checkingSliderInterval);
      setTimeout(() => {
        flkty = new Flickity(".swiper-wrapper", {
          cellAlign: "left",
          contain: true,
          // freeScroll: true,
          // percentPosition: true,
          pageDots: false,
          wrapAround: true,
          autoPlay: 2000,
          // prevNextButtons: false,
          draggable: true
        });
        let currentSlide = $(".swiper-slide.is-selected").index();
        $("#slider-next-btn").append(
          $(".flickity-button.flickity-prev-next-button.next")
        );
        $("#slider-prev-btn").append(
          $(".flickity-button.flickity-prev-next-button.previous")
        );
        flkty.on("change", function () {
          console.log("slide changed ", currentSlide);

          // updatePageHeight = true;
          $("#slider-prev-btn").css("pointer-events", "auto");
          $("#slider-prev-btn").addClass("is--active");
        });
        flkty.stopPlayer();
      }, 250);
    }
  }, 200);
  const isIpadOS = () => {
    return (
      navigator.maxTouchPoints &&
      navigator.maxTouchPoints > 2 &&
      /MacIntel/.test(navigator.platform)
    );
  };

  console.log(window.innerWidth);

  init();
  function init() {
    if (window.innerWidth > 992 && window.innerWidth < 1250) {
      $("body").addClass("ipad-mode");
    }
    if (isIpadOS() === 1) {
      console.log("ipad");
      $("body").addClass("ipad-mode");
    }
    if (window.innerWidth > 1250 && isIpadOS() === 0) {
      windowViewPort = "desktop";
    } else if (window.innerWidth > 1250 && isIpadOS() === 1) {
      windowViewPort = "tablet";
    } else if (window.innerWidth <= 1250 && window.innerWidth > 767) {
      windowViewPort = "tablet";
    } else {
      windowViewPort = "mobile";
    }
    let userAgent = navigator.userAgent;
    let browserName;

    if (userAgent.match(/chrome|chromium|crios/i)) {
      browserName = "chrome";
    } else if (userAgent.match(/firefox|fxios/i)) {
      browserName = "firefox";
    } else if (userAgent.match(/safari/i)) {
      browserName = "safari";
    } else if (userAgent.match(/opr\//i)) {
      browserName = "opera";
    } else if (userAgent.match(/edg/i)) {
      browserName = "edge";
    } else {
      browserName = "No browser detection";
    }

    var OperatingSystemName = "Not known";
    if (navigator.appVersion.indexOf("Win") !== -1)
      OperatingSystemName = "Windows OS";
    if (navigator.appVersion.indexOf("Mac") !== -1)
      OperatingSystemName = "MacOS";
    if (navigator.appVersion.indexOf("X11") !== -1)
      OperatingSystemName = "UNIX OS";
    if (navigator.appVersion.indexOf("Linux") !== -1)
      OperatingSystemName = "Linux OS";

    if (windowViewPort === "desktop") {
      $(".section.footer").each(function (index) {
        let triggerElement = $(this);

        let tl = gsap.timeline({
          scrollTrigger: {
            trigger: triggerElement,
            start: "top bottom",
            end: "bottom bottom",
            onEnter: () => {
              if (updatePageHeight === true) {
                console.log("ScrollTrigger refreshed");
                updatePageHeight = false;
              }
            }
          }
        });
      });
    }

    setTimeout(() => {
      gsap.to(".home-hero-background", {
        opacity: 1,
        duration: 0.85
      });
    }, 750);
    // alert(window.innerWidth);
    let heroPhoneVideo = $(".embed-video.phone video");
    let lightWavesVideo = $(".embed-video.light-waves video");
    let phoneVideoSrc = null;
    let wavesVideoSrc = null;
    if (OperatingSystemName === "MacOS") {
      if (windowViewPort === "desktop") {
        phoneVideoSrc =
          browserName === "safari"
            ? "https://raw.githubusercontent.com/Khai-12345/lux-algo-storage/main/Lux%20Algo/videos/Phone%20v3.mov"
            : "https://raw.githubusercontent.com/Khai-12345/lux-algo-storage/main/Lux%20Algo/videos/Phone%20v3.webm";
        // ? "https://dreamy-bombolone-48048e.netlify.app/videos/phone-animation-hevc.mov"
        // : "https://dreamy-bombolone-48048e.netlify.app/videos/phone.webm";
        wavesVideoSrc =
          browserName === "safari"
            ? "https://raw.githubusercontent.com/Khai-12345/lux-algo-storage/main/Lux%20Algo/videos/light-waves-hecv.mov"
            : "https://raw.githubusercontent.com/Khai-12345/lux-algo-storage/main/Lux%20Algo/videos/light-waves-compressed.webm";
        // ? "https://dreamy-bombolone-48048e.netlify.app/videos/light-waves-hecv.mov"
        // : "https://dreamy-bombolone-48048e.netlify.app/videos/light-waves-compressed.webm";
      } else {
        // phoneVideoSrc =
        //   "https://raw.githubusercontent.com/Khai-12345/lux-algo-storage/main/Lux%20Algo/videos/Phone-v14-09-hevc.mov";
        // wavesVideoSrc =
        //   "https://raw.githubusercontent.com/Khai-12345/lux-algo-storage/main/Lux%20Algo/videos/light-waves-hecv.mov";
      }
    } else {
      phoneVideoSrc =
        "https://raw.githubusercontent.com/Khai-12345/lux-algo-storage/main/Lux%20Algo/videos/Phone%20v3.webm";
      wavesVideoSrc =
        "https://raw.githubusercontent.com/Khai-12345/lux-algo-storage/main/Lux%20Algo/videos/light-waves-compressed.webm";
      // phoneVideoSrc =
      //   "https://dreamy-bombolone-48048e.netlify.app/videos/phone.webm";
      // wavesVideoSrc =
      //   "https://dreamy-bombolone-48048e.netlify.app/videos/light-waves-compressed.webm";
    }
    // setTimeout(() => {
    heroPhoneVideo.attr("src", `${phoneVideoSrc}`);
    lightWavesVideo.attr("src", `${wavesVideoSrc}`);
    ///Hero video
    heroPhoneVideo.get(0).pause();
    lightWavesVideo.get(0).pause();
    // }, 250);

    // $(".scrub-section").each(function (index) {
    //   let triggerElement = $(this);

    //   let tl = gsap.timeline({
    //     scrollTrigger: {
    //       trigger: triggerElement,
    //       start: "top top",
    //       end: "bottom bottom",
    //       toggleActions: "play none none reverse",
    //       pin: $(this).find(".home-hero-video")
    //       // onEnter: () => {}
    //     }
    //   });
    // });
    // if (windowViewPort === "desktop") {
    $(".hero-video-phone").each(function (index) {
      let triggerElement = $(this);

      let tl = gsap.timeline({
        scrollTrigger: {
          trigger: triggerElement,
          start: `top ${windowViewPort === "desktop" ? "10%" : "100%"}`,
          end: "bottom bottom",
          // toggleActions: "play none none none",
          // markers: true,
          once: true,
          onEnter: () => {
            gsap
              .timeline()
              .fromTo(
                ".home-hero-lower",
                {
                  opacity: 0,
                  y: "0.3rem"
                },
                {
                  opacity: 1,
                  y: "0rem"
                }
              )
              .to(".embed-video.light-waves", {
                opacity: 1,
                duration: 1,
                ease: "power1.inOut"
              });
            setTimeout(() => {
              heroPhoneVideo.get(0).play();
              lightWavesVideo.get(0).play();
            }, 300);
          }
        }
      });
    });

    //Animate gradients in the background

    let heroGradientContainer = $(
      ".home-hero-gradients-contain > .position-relative"
    );
    let heroGradient1 = $(".home-hero-gradients-contain .abstract-gradient._1");
    let heroGradient2 = $(".home-hero-gradients-contain .abstract-gradient._2");
    let heroGradient3 = $(".home-hero-gradients-contain .abstract-gradient._3");
    let heroGradientsMoving = gsap.timeline({
      repeat: -1,
      repeatDelay: 0,
      paused: false
    });
    heroGradientsMoving
      .set(heroGradient2, {
        opacity: 0
      })
      .set(heroGradient3, {
        opacity: 0
      })
      .to(heroGradientContainer, {
        rotateZ: 360,
        duration: 45
      });

    let heroGradientsPulsing = gsap
      .timeline({ repeat: -1, repeatDelay: 0, paused: false })
      .to(
        heroGradient1,
        {
          opacity: 0,
          duration: 4
        },
        0
      )
      .to(
        heroGradient2,
        {
          opacity: 1,
          duration: 4
        },
        0
      )
      .to(
        heroGradient2,
        {
          opacity: 0,
          duration: 4
        },
        4
      )
      .to(
        heroGradient3,
        {
          opacity: 1,
          duration: 4
        },
        4
      )
      .to(
        heroGradient3,
        {
          opacity: 0,
          duration: 4
        },
        8
      )
      .to(
        heroGradient1,
        {
          opacity: 1,
          duration: 4
        },
        8
      );

    let updatePageHeight = false;

    // Set image to cover
    function drawImageProp(ctx, img, x, y, w, h, offsetX, offsetY) {
      if (arguments.length === 2) {
        x = y = 0;
        w = ctx.canvas.width;
        h = ctx.canvas.height;
      }
      offsetX = typeof offsetX === "number" ? offsetX : 0.5;
      offsetY = typeof offsetY === "number" ? offsetY : 0.5;
      if (offsetX < 0) offsetX = 0;
      if (offsetY < 0) offsetY = 0;
      if (offsetX > 1) offsetX = 1;
      if (offsetY > 1) offsetY = 1;
      var iw = img.width,
        ih = img.height,
        r = Math.min(w / iw, h / ih),
        nw = iw * r,
        nh = ih * r,
        cx,
        cy,
        cw,
        ch,
        ar = 1;
      if (nw < w) ar = w / nw;
      if (Math.abs(ar - 1) < 1e-14 && nh < h) ar = h / nh; // updated
      nw *= ar;
      nh *= ar;
      cw = iw / (nw / w);
      ch = ih / (nh / h);
      cx = (iw - cw) * offsetX;
      cy = (ih - ch) * offsetY;
      if (cx < 0) cx = 0;
      if (cy < 0) cy = 0;
      if (cw > iw) cw = iw;
      if (ch > ih) ch = ih;
      ctx.drawImage(img, cx, cy, cw, ch, x, y, w, h);
    }

    // Apply interaction to all elements with this class
    if (windowViewPort === "desktop") {
      $(".scrub-section").each(function (index) {
        const canvas = $(this).find("canvas")[0];
        const embed = $(this).find(".embed")[0];
        const context = canvas.getContext("2d");
        function setCanvasSize() {
          canvas.width = embed.offsetWidth;
          canvas.height = embed.offsetHeight;
        }
        setCanvasSize();
        const frameCount = $(this).attr("total-frames");
        const urlStart = $(this).attr("url-start");
        const urlEnd = $(this).attr("url-end");
        const floatingZeros = $(this).attr("floating-zeros");
        const currentFrame = (index) =>
          `${urlStart}${(index + 1)
            .toString()
            .padStart(floatingZeros, "0")}${urlEnd}`;
        const images = [];
        const imageFrames = {
          frame: 0
        };
        for (let i = 0; i < frameCount; i++) {
          const img = new Image();
          img.src = currentFrame(i);
          images.push(img);
        }
        gsap.to(imageFrames, {
          frame: frameCount - 1,
          snap: "frame",
          ease: "none",
          scrollTrigger: {
            trigger: $(this),
            start: $(this).attr("scroll-start"),
            end: $(this).attr("scroll-end"),
            scrub: 0
          },
          onUpdate: render
        });
        images[0].onload = render;
        function render() {
          context.clearRect(0, 0, embed.offsetWidth, embed.offsetHeight);
          drawImageProp(
            context,
            images[imageFrames.frame],
            0,
            0,
            embed.offsetWidth,
            embed.offsetHeight,
            0.5,
            0.5
          );
          // requestAnimationFrame(render); ////// Might help to improve performance
        }

        // Update canvas size & animation state
        let iOS = !!navigator.platform.match(/iPhone|iPod|iPad/);
        let resizeTimer;
        $(window).on("resize", function (e) {
          if (iOS) {
            clearTimeout(resizeTimer);
            resizeTimer = setTimeout(function () {
              setCanvasSize();
              render();
            }, 250);
          } else {
            setCanvasSize();
            render();
          }
        });
        setTimeout(() => {
          ScrollTrigger.refresh();
        }, 300);
      });
    }

    setTimeout(() => {
      $(".home-hero-upper").each(function (index) {
        let triggerElement = $(this);

        let tl = gsap.timeline({
          scrollTrigger: {
            trigger: triggerElement,
            start: "top 60%",
            end: "35% top",
            toggleActions: "play none none reverse",
            // once: true
            onEnter: () => {
              gsap
                .timeline()
                .fromTo(
                  ".home-hero-upper",
                  {
                    opacity: 0,
                    y: "1rem"
                  },
                  {
                    opacity: 1,
                    y: "0rem",
                    duration: 1.35,
                    ease: "circ.out"
                  }
                )
                .to(".home-coins-img-holder", {
                  opacity: 1
                });
            },
            onLeave: () => {}
          }
        });
      });
    }, 450);

    if (windowViewPort !== "desktop") {
      console.log("not desktop");
      //Add fade-in amationa for tablet and mobile
      setTimeout(() => {
        gsap.to(
          ".hero-video-phone,.home-hero-lightwaves-img, .home-phone-img-holder ",
          {
            opacity: 1
          }
        );
      }, 850);
      $(
        `.big-card-coin,
      .container.testimonial .heading-xl,
      .swiper-wrapper,
      .container.testimonial .heading-l,
      .container.testimonial .grid-3-col,
      #plan-section .heading-xl,
      .benefit-check-list,
      .price-card,
      #payment-options,
      .faq-item,
      #faq-heading,
      .cta-container,
      .home-count-down`
      ).each(function (index) {
        let triggerElement = $(this);

        let tl = gsap.timeline({
          scrollTrigger: {
            trigger: triggerElement,
            start: "top 90%",
            end: "top 80%",
            toggleActions: "play none none none"
            // markers: true
            // once: true
            // onEnter: () => {}
          }
        });
        tl.from($(this), {
          opacity: 0,
          duration: 1.15,
          delay: 0.1,
          y: "0.5rem"
        });
      });
    }
    if (window.innerWidth < 1250) {
      $(".home-big-card-screen").each(function (index) {
        let triggerElement = $(this);

        let tl = gsap.timeline({
          scrollTrigger: {
            trigger: triggerElement,
            start: "top 85%",
            end: "bottom 75%",
            once: true
          }
        });
        tl.fromTo(
          triggerElement,
          {
            opacity: 0,
            y: "0.5rem"
          },
          {
            opacity: 1,
            duration: 1.15,
            delay: 0.1,
            y: "0rem"
          }
        );
      });
    }

    if (windowViewPort === "desktop") {
      //Plans packages
      $(".price-card-container").each(function (index) {
        let triggerElement = $(this);

        let tl = gsap.timeline({
          scrollTrigger: {
            trigger: triggerElement,
            start: "top 90%",
            end: "top 80%",
            toggleActions: "play none none none"
            // once: true
            // onEnter: () => {}
          }
        });
        tl.from($("#plan-section .heading-xl"), {
          opacity: 0,
          x: "-5rem"
        })
          .from(
            $(".benefit-check-list-item"),
            {
              x: "1rem",
              opacity: 0,
              stagger: { each: 0.35, from: "start" },
              ease: "power2.inOut",
              duration: 0.85
              // duration: 1
            },
            // "<"
            ">-0.95"
          )
          // .from(
          //   $(".home-count-down"),
          //   {
          //     opacity: 0,
          //     ease: "power2.inOut",
          //     duration: 1
          //   },
          //   ">-0.5"
          // )
          .from(
            $(this).find(".price-card"),
            {
              y: "3.5rem",
              opacity: 0,
              stagger: { each: 0.35, from: "start" },
              ease: "power2.inOut",
              duration: 1
              // duration: 1
            },
            "<"
          )
          .from($(".premium-card-bg"), {
            opacity: 0,
            duration: 0.65
          })
          .from($(".price-card-plan.black"), {
            backgroundColor: "#2b3345"
          });
        // .from(
        //   $(".benefit-check-list-item"),
        //   {
        //     x: "1rem",
        //     opacity: 0,
        //     stagger: { each: 0.35, from: "start" },
        //     ease: "power2.inOut",
        //     duration: 0.85
        //     // duration: 1
        //   },
        //   // "<"
        //   ">-2.35"
        // )
      });
    }
    let paymentOptionsContainer = $("#payment-options");
    let paymentOptionsHeading = paymentOptionsContainer.find(".heading-l");
    let paymentOptionsRight = paymentOptionsContainer.find("._w-600");

    if (windowViewPort === "desktop") {
      let paymentOptionAnimation = gsap.timeline(
        {
          scrollTrigger: {
            trigger: paymentOptionsContainer,
            start: "top 95%",
            end: "top 85%",
            toggleActions: "play none none reverse"
          }
        },
        0
      );
      paymentOptionAnimation
        .from(paymentOptionsHeading, {
          opacity: 0
        })
        .from(paymentOptionsRight, {
          opacity: 0,
          x: "2rem"
        });
    }
    //Animate gradients in the background

    let plansGradientContainer = $(
      ".plans-gradient-group > .position-relative"
    );
    let plansGradient1 = $("#plan-section .abstract-gradient._1");
    let plansGradient2 = $("#plan-section .abstract-gradient._2");
    let plansGradient3 = $("#plan-section .abstract-gradient._3");
    let planGradientsMoving = gsap.timeline({
      repeat: -1,
      repeatDelay: 0,
      paused: false
    });
    planGradientsMoving
      .set(plansGradient2, {
        opacity: 0
      })
      .set(plansGradient3, {
        opacity: 0
      })
      .to(plansGradientContainer, {
        rotateZ: 360,
        duration: 45
      });

    let gradientsPulsing = gsap
      .timeline({ repeat: -1, repeatDelay: 0, paused: false })
      .to(
        plansGradient1,
        {
          opacity: 0,
          duration: 4
        },
        0
      )
      .to(
        plansGradient2,
        {
          opacity: 1,
          duration: 4
        },
        0
      )
      .to(
        plansGradient2,
        {
          opacity: 0,
          duration: 4
        },
        4
      )
      .to(
        plansGradient3,
        {
          opacity: 1,
          duration: 4
        },
        4
      )
      .to(
        plansGradient3,
        {
          opacity: 0,
          duration: 4
        },
        8
      )
      .to(
        plansGradient1,
        {
          opacity: 1,
          duration: 4
        },
        8
      );

    //FAQs
    $(".faq-item").on("click", function () {
      let itemContent = $(this).find(".faq-item-content");
      let itemSpace = $(this).find(".faq-space");
      let faqIcon = $(this).find(".accordion-icon");
      //check if the item is being open or close
      if ($(this).hasClass("is--active")) {
        $(this).css("background-color", "#202737");
        itemContent.css("max-height", "0rem");
        itemContent.css("opacity", "0");
      } else {
        itemContent.css("max-height", "20rem");
        $(this).css("background-color", "#2B3345");
        itemContent.css("opacity", "1");
      }
      itemSpace.toggleClass("is--passive");
      faqIcon.toggleClass("is--passive");
      $(this).toggleClass("is--active");
      updatePageHeight = true;
    });

    if (windowViewPort === "desktop") {
      $(".faq-container").each(function (index) {
        let triggerElement = $(this);

        let tl = gsap.timeline({
          scrollTrigger: {
            trigger: triggerElement,
            start: "top 90%",
            end: "top 80%",
            toggleActions: "play none none reverse"
            // once: true
          }
        });
        tl.from($("#faq-heading"), {
          opacity: 0
        }).from(
          $(this).find(".faq-item"),
          {
            y: "2rem",
            opacity: 0,
            stagger: { each: 0.3, from: "start" },
            ease: "power2.inOut",
            duration: 1.1
            // duration: 1
          },
          0.3
        );
      });
    }

    // Change background from black to white when entering testomonial section
    setTimeout(() => {
      $(".container.regualr-padding.testimonial").each(function (index) {
        let triggerElement = $(this);

        let tl = gsap.timeline({
          scrollTrigger: {
            trigger: triggerElement,
            start: "top 55%",
            end: "top 30%",
            toggleActions: "play none none none"
          }
        });
        tl.from($(this), {
          backgroundColor: "#131722",
          // duration: 0.7,
          ease: "linear"
        })
          .from(
            $(this).find(".heading-xl,.heading-m,.p-jumbo,.heading-s"),
            {
              color: "white",
              // duration: 0.7,
              ease: "linear"
            },
            "<"
          )
          .from(
            $(this).find(".customer-review-item"),
            {
              backgroundColor: "black",
              // duration: 0.7,
              ease: "linear"
            },
            "<"
          )
          .add(() => flkty.playPlayer());
      });
    }, 2350);
    ///Make the card sticky to the top of the screeen while scrolling inside home card container

    if (windowViewPort === "desktop") {
      $(".card-sticky-container").each(function (index) {
        let triggerElement = $(".home-card-container");

        let tl = gsap.timeline({
          scrollTrigger: {
            trigger: triggerElement,
            start: "top top",
            end: "bottom bottom",
            // pin: $(this),
            // scrub: 2,
            toggleActions: "play none none reverse"
          }
        });
      });
    }

    if (windowViewPort !== "desktop") {
      $(".home-big-card-screen._1").each(function (index) {
        let triggerElement = $(this);

        let tl = gsap.timeline({
          scrollTrigger: {
            trigger: triggerElement,
            start: "top 20%",
            end: "bottom bottom",
            toggleActions: "play none none none"
            // markers: true
          }
        });
        tl.to(".home-chart-screen-1._2", {
          opacity: 1
        });
      });
      $(".home-big-card-screen._3").each(function (index) {
        let triggerElement = $(this);

        let tl = gsap.timeline({
          scrollTrigger: {
            trigger: triggerElement,
            start: "top 20%",
            end: "bottom bottom",
            toggleActions: "play none none none"
            // markers: true
          }
        });
        tl.to(".home-chart-screen-3._2.no1", {
          delay: 0.2,
          opacity: 1,
          duration: 1.5,
          ease: "power2.inOut"
        })
          .to(".home-chart-screen-3._2.no2", {
            delay: 0.2,
            opacity: 1,
            duration: 1.5,
            ease: "power2.inOut"
          })
          .to(".home-chart-screen-3._2.no3", {
            delay: 0.2,
            opacity: 1,
            duration: 1.5,
            ease: "power2.inOut"
          });
      });
    }
    //Animate the card container
    if (windowViewPort === "desktop") {
      $(".home-big-card-screen").each(function (index) {
        let triggerElement = $(this);
        let cardScreen = $(".home-big-card-screen").eq(index);
        let cardIcons = cardScreen.find(".big-card-coin");
        // let animationStartPositions = index === 0 ? "0% 70%" : "0% 20%";
        let card = cardScreen.find(".home-big-card");
        let cardBG = card.find(".home-big-card-bg");
        let cardContent = card.find(".home-big-card-content");

        let tl = gsap.timeline({
          scrollTrigger: {
            trigger: triggerElement,
            start: "0% 60%",
            end: "0% 90%",
            // scrub: true,
            // markers: true,
            toggleActions: "play none none none",
            once: true,
            onEnter: () => {
              console.log("Entering card " + index, cardScreen.css("opacity"));
              if (index === 0) {
                gsap
                  .timeline()
                  .to(
                    ".home-chart-screen-1._1",
                    {
                      opacity: 1,
                      x: "0rem"
                    },
                    0
                  )
                  .to(
                    ".home-chart-screen-1._2",
                    {
                      opacity: 1,
                      duration: 1.5,
                      ease: "power2.inOut"
                    },
                    0.8
                  )
                  .to(
                    ".home-card-blur",
                    {
                      opacity: 1,
                      scale: 1
                    },
                    1.5
                  );
              } else if (index === 1) {
                gsap
                  .timeline()
                  .to(
                    ".home-chart-screen-2._1",
                    {
                      opacity: 1,
                      x: "0rem"
                    },
                    0
                  )
                  .to(
                    ".home-chart-screen-2._2",
                    {
                      opacity: 1,
                      x: "0rem"
                    },
                    0.7
                  )
                  .to(
                    ".home-card-blur-2",
                    {
                      opacity: 1,
                      scale: 1
                    },
                    1.7
                  );
              } else {
                gsap
                  .timeline()
                  .to(
                    ".home-chart-screen-3._1",
                    {
                      opacity: 1,
                      x: "0"
                    },
                    0
                  )
                  .fromTo(
                    ".home-card-blur-3",
                    {
                      x: "30rem"
                    },
                    {
                      delay: 0.5,
                      x: "0rem",
                      opacity: 1,
                      scale: 1
                    },
                    "<"
                  )
                  .to(
                    ".home-chart-screen-3._2.no1",
                    {
                      delay: 0.5,
                      opacity: 1,
                      duration: 1.5,
                      ease: "power2.inOut"
                    },
                    "<"
                    // 0.8
                  )
                  .to(
                    ".home-chart-screen-3._2.no2",
                    {
                      delay: 0.5,
                      opacity: 1,
                      duration: 1.5,
                      ease: "power2.inOut"
                    }
                    // 0.8
                  )
                  .to(
                    ".home-chart-screen-3._2.no3",
                    {
                      delay: 0.5,
                      opacity: 1,
                      duration: 1.5,
                      ease: "power2.inOut"
                    }
                    // 0.8
                  );
              }
            }
          }
        });
        tl.to(cardScreen, {
          duration: 0.5
        }).to(cardIcons, {
          opacity: 1,
          duration: 0.85
        });
      });
    }
  }

  if (windowViewPort === "mobile") {
    if (
      $(".header-announcement").length !== 0 &&
      $(".header-announcement").css("display") !== "none"
    ) {
      $(
        `.hamburger-icons-contain,.header-nav-link:not([is-title="true"]),.tablet-dropdown-menu-item:not([is-title="true"]),[is-buy-now-btn="true"]`
      ).on("click", function () {
        let distance = $(".header-announcement").innerHeight();
        console.log(distance);
        // Check if the mobile menu is being open
        if (!mobileNavClosed) {
          $(".header-upper").css("transform", `translateY(${distance * -1}px)`);
        } else {
          $(".header-upper").css("transform", `translateY(0px)`);
        }
      });
    }
  }

  /////Countdown script

  // function changeTimeZone(date, timeZone) {
  //   if (typeof date === "string") {
  //     return new Date(
  //       new Date(date).toLocaleString("en-US", {
  //         timeZone
  //       })
  //     );
  //   }

  //   return new Date(
  //     date.toLocaleString("en-US", {
  //       timeZone
  //     })
  //   );
  // }
  // document.addEventListener("DOMContentLoaded", () => {
  //   // const offer_due_date = new Date("11/29/2022 5:00:00 AM UTC");

  //   let now_local = new Date();
  //   let now_dubai = changeTimeZone(new Date(), "Asia/Dubai");
  //   let now_local_second = now_local.getTime() / 1000;
  //   let now_dubai_second = now_dubai.getTime() / 1000;
  //   let second_diff = now_local_second - now_dubai_second;
  //   let offer_due_date_local_second =
  //     new Date("11/29/2022 09:00:00 am").getTime() / 1000;
  //   let correct_value;
  //   if (second_diff > 0) {
  //     correct_value = offer_due_date_local_second + second_diff;
  //   } else {
  //     correct_value = offer_due_date_local_second + second_diff;
  //   }

  //   // let now_Toronto = changeTimeZone(new Date(), "Canada/Eastern");

  //   console.log(
  //     "local",
  //     now_local_second,
  //     "target",
  //     now_dubai_second,
  //     "diff",
  //     second_diff,
  //     "due time target",
  //     correct_value
  //   );

  //   // Set up FlipDown
  //   var flipdown = new FlipDown(correct_value)

  //     // Start the countdown
  //     .start()

  //     // Do something when the countdown ends
  //     .ifEnded(() => {
  //       console.log("The countdown has ended!");
  //     });
  // });
});
