<!-- This is a Vue.js single file component. -->
<!-- Check the Vue.js doc here :  -->
<!-- https://vuejs.org/v2/guide/ -->

<!-- This is your HTML -->
<template>
    <div class="blog-article-curved-header">
        <!-- wwManager:start -->
        <wwSectionEditMenu v-bind:sectionCtrl="sectionCtrl" :options="openOptions"></wwSectionEditMenu>
        <!-- wwManager:end -->
        <wwObject class="background" v-bind:ww-object="section.data.color" ww-category="background"></wwObject>

        <div class="header">
            <wwObject class="image" ww-category="background" v-bind:ww-object="section.data.header"></wwObject>
        </div>
        <div class="row-container">
            <div class="left">
                <wwObject class="show-mobile" v-bind:ww-object="section.data.btnLeftMobile"></wwObject>
                <wwObject class="show-desktop" v-if="!isMobile" v-bind:ww-object="section.data.btnLeft"></wwObject>
            </div>
            <div v-if="displayShareLinks" class="right">
                <span class="show-mobile">
                    <wwObject v-bind:ww-object="section.data.share" @click="toggleDisplaySn()"></wwObject>
                    <div class="icon-container" :style="{'display': displaySn}">
                        <!-- LinkedIn -->
                        <wwObject v-if="section.data.linkedin" class="icon" v-bind:ww-object="section.data.icon1" @click="shareOn('linkedin')"></wwObject>
                        <!-- Twitter -->
                        <wwObject v-if="section.data.twitter" class="icon" v-bind:ww-object="section.data.icon2" @click="shareOn('twitter')"></wwObject>
                        <!-- Facebook -->
                        <wwObject v-if="section.data.facebook" class="icon" v-bind:ww-object="section.data.icon3" @click="shareOn('facebook')"></wwObject>
                    </div>
                </span>
                <span class="show-desktop">
                    <!-- LinkedIn -->
                    <div v-if="section.data.linkedin" class="icon">
                        <wwObject v-bind:ww-object="section.data.icon1" @click="shareOn('linkedin')"></wwObject>
                        <div class="tooltip" :class="{'force-display': editMode}">
                            <wwObject v-bind:ww-object="section.data.icon1Tip"></wwObject>
                        </div>
                    </div>
                    <!-- Twitter -->
                    <div v-if="section.data.twitter" class="icon">
                        <wwObject v-bind:ww-object="section.data.icon2" @click="shareOn('twitter')"></wwObject>
                        <div class="tooltip" :class="{'force-display': editMode}">
                            <wwObject v-bind:ww-object="section.data.icon2Tip"></wwObject>
                        </div>
                    </div>
                    <!-- Facebook -->
                    <div v-if="section.data.facebook" class="icon">
                        <wwObject v-bind:ww-object="section.data.icon3" @click="shareOn('facebook')"></wwObject>
                        <div class="tooltip" :class="{'force-display': editMode}">
                            <wwObject v-bind:ww-object="section.data.icon3Tip"></wwObject>
                        </div>
                    </div>
                </span>
            </div>
        </div>
        <div class="contents">
            <wwLayoutColumn tag="div" ww-default="ww-text" :ww-list="section.data.contents" class="content" @ww-add="add(section.data.contents, $event)" @ww-remove="remove(section.data.contents, $event)">
                <wwObject v-for="content in section.data.contents" :key="content.uniqueId" :ww-object="content"></wwObject>
            </wwLayoutColumn>
        </div>
    </div>
</template>

<!-- This is your Javascript -->
<!-- ✨ Here comes the magic ✨ -->
<script>
export default {
    name: "__COMPONENT_NAME__",
    props: {
        // The section controller object is passed to you.
        sectionCtrl: Object
    },
    data() {
        return {
            showSn: false
        }
    },
    computed: {
        //Get the section object here !
        // It contains all the info and data about the section
        // Use it has you like !
        section() {
            return this.sectionCtrl.get();
        },
        editMode() {
            return this.sectionCtrl.getEditMode() == 'CONTENT'
        },
        displaySn() {
            if (this.showSn) {
                return 'initial'
            } else {
                return 'none'
            }
        },
        displayShareLinks() {
            return (this.section.data.linkedin || this.section.data.twitter || this.section.data.facebook) ? true : false
        }
    },
    created() {

        //Initialize section data
        let needUpdate = false

        this.section.data = this.section.data || {};

        //Initialize content
        if (!this.section.data.color) {
            this.section.data.color = wwLib.wwObject.getDefault({
                type: 'ww-color'
            });
        }

        if (!this.section.data.header) {
            this.section.data.header = wwLib.wwObject.getDefault({
                type: 'ww-image'
            });
        }

        if (!this.section.data.btnLeft) {
            this.section.data.btnLeft = wwLib.wwObject.getDefault({
                type: 'ww-button'
            });
            needUpdate = true
        }

        if (!this.section.data.btnLeftMobile) {
            this.section.data.btnLeftMobile = wwLib.wwObject.getDefault({
                type: 'ww-button'
            });
            needUpdate = true
        }

        if (!this.section.data.icon1) {
            this.section.data.icon1 = wwLib.wwObject.getDefault({
                type: 'ww-icon'
            });
            needUpdate = true
        }

        if (!this.section.data.icon1Tip) {
            this.section.data.icon1Tip = wwLib.wwObject.getDefault({
                type: 'ww-text'
            });
            needUpdate = true
        }

        if (!this.section.data.icon2) {
            this.section.data.icon2 = wwLib.wwObject.getDefault({
                type: 'ww-icon'
            });
            needUpdate = true
        }

        if (!this.section.data.icon2Tip) {
            this.section.data.icon2Tip = wwLib.wwObject.getDefault({
                type: 'ww-text'
            });
            needUpdate = true
        }

        if (!this.section.data.icon3) {
            this.section.data.icon3 = wwLib.wwObject.getDefault({
                type: 'ww-icon'
            });
            needUpdate = true
        }

        if (!this.section.data.icon3Tip) {
            this.section.data.icon3Tip = wwLib.wwObject.getDefault({
                type: 'ww-text'
            });
            needUpdate = true
        }

        if (!this.section.data.share) {
            this.section.data.share = wwLib.wwObject.getDefault({
                type: 'ww-icon'
            });
            needUpdate = true
        }

        if (typeof this.section.data.linkedin == 'undefined') {
            this.section.data.linkedin = true
            needUpdate = true
        }

        if (typeof this.section.data.facebook == 'undefined') {
            this.section.data.facebook = true
            needUpdate = true
        }

        if (typeof this.section.data.twitter == 'undefined') {
            this.section.data.twitter = true
            needUpdate = true
        }

        if (_.isEmpty(this.section.data.contents)) {
            this.section.data.contents = [];
            needUpdate = true;
        }

        if (needUpdate) {
            this.sectionCtrl.update(this.section);
        }

    },
    methods: {
        toggleDisplaySn() {
            this.showSn = !this.showSn;
        },
        shareOn(socialMedia) {
            const url = window.location.href;
            const page = wwLib.wwWebsiteData.getCurrentPage()
            const title = wwLib.wwLang.getText(page.title)
            switch (socialMedia) {
                case 'linkedin':
                    window.open(`https://www.linkedin.com/shareArticle?mini=true&url=${url}', '_blank'`);
                    break;
                case 'twitter':
                    window.open(`https://twitter.com/share?url=${url}&amp;text=${title} &amp;', '_blank'`);
                    break;
                case 'facebook':
                    window.open(`https://www.facebook.com/sharer.php?u=${url} title=${title}, '_blank'`);
                    break;

                default:
                    break;
            }


        },
        /* wwManager:start */
        add(list, options) {
            try {
                list.splice(options.index, 0, options.wwObject);
                this.sectionCtrl.update(this.section);
            } catch (error) {
                wwLib.wwLog.error('ERROR : ', error);
            }
        },
        remove(list, options) {
            try {
                list.splice(options.index, 1);
                this.sectionCtrl.update(this.section);
            } catch (error) {
                wwLib.wwLog.error('ERROR : ', error);
            }
        },
        async openOptions() {
            try {
                wwLib.wwPopups.addStory('BLOG_ARTICLE_CONFIG', {
                    title: {
                        en: 'Article configuration',
                        fr: 'Configuration de l\'article'
                    },
                    type: 'wwPopupForm',
                    storyData: {
                        fields: [
                            {
                                label: {
                                    en: 'Share on LinkedIn button:',
                                    fr: 'Bouton partager sur LinkedIn :'
                                },
                                type: 'radio',
                                key: 'linkedin',
                                valueData: 'section.data.linkedin'
                            },
                            {
                                label: {
                                    en: 'Share on Facebook button:',
                                    fr: 'Bouton partager sur Facebook :'
                                },
                                type: 'radio',
                                key: 'facebook',
                                valueData: 'section.data.facebook'
                            },
                            {
                                label: {
                                    en: 'Share on Twitter button:',
                                    fr: 'Bouton partager sur Twitter :'
                                },
                                type: 'radio',
                                key: 'twitter',
                                valueData: 'section.data.twitter'
                            },
                        ]
                    },
                    buttons: {
                        NEXT: {
                            text: {
                                en: 'Finish',
                                fr: 'Terminer'
                            },
                            next: null
                        }
                    }
                })
                let options = {
                    firstPage: 'BLOG_ARTICLE_CONFIG',
                    data: {
                        section: this.section,
                    },
                }
                const result = await wwLib.wwPopups.open(options)
                let needUpdate = false
                if (typeof (result) != 'undefined') {
                    if (typeof (result.linkedin) != 'undefined') {
                        this.section.data.linkedin = result.linkedin
                        needUpdate = true
                    }
                    if (typeof (result.twitter) != 'undefined') {
                        this.section.data.twitter = result.twitter
                        needUpdate = true
                    }
                    if (typeof (result.facebook) != 'undefined') {
                        this.section.data.facebook = result.facebook
                        needUpdate = true
                    }
                    if (needUpdate) {
                        this.sectionCtrl.update(this.section);
                        this.$forceUpdate();
                    }
                }
            } catch (error) {
                wwLib.wwLog.error('ERROR : ', error);
            }
        },
        /* wwManager:end */

    }
};
</script>

<!-- This is your CSS -->
<!-- Add "scoped" attribute to limit CSS to this component only -->
<!-- Add lang="scss" or others to use computed CSS -->
<style lang="scss" scoped>
$roundWidth: 200px;
$margin: 100px;
.blog-article-curved-header {
    .svg {
        width: 100%;
        position: relative;
    }
    .background {
        position: absolute;
        top: 0;
        left: 0;
        height: 100%;
        width: 100%;
    }

    .header {
        position: relative;
        width: 100%;
        height: 180px;
        border-bottom-left-radius: 50%;
        border-bottom-right-radius: 50%;
        clip-path: ellipse(70% 70% at 50% 30%);
        @media (min-width: 768px) {
            height: 250px;
        }
        @media (min-width: 992px) {
            height: 300px;
        }
        @media (min-width: 1200px) {
            height: 400px;
        }
        .image {
            position: absolute;
            top: 0;
            left: 0;
            height: 100%;
            width: 100%;
        }
    }
    .row-container {
        position: relative;
        display: flex;
        justify-content: space-between;
        width: 100%;
        padding-top: 20px;
        margin-bottom: 20px;
        // overflow: initial;
        .round {
            position: absolute;
            left: 50%;
            top: 0;
            width: 100px;
            height: 100px;
            transform: translate(-50%, -50%);

            @media (min-width: 768px) {
                width: 150px;
                height: 150px;
            }
            @media (min-width: 992px) {
                width: 180px;
                height: 180px;
            }
            @media (min-width: 1200px) {
                width: 200px;
                height: 200px;
            }
        }
        .left,
        .right {
            width: calc(50% - 50px);
            display: flex;
            justify-content: center;

            @media (min-width: 768px) {
                width: calc(50% - 70px);
            }
            @media (min-width: 992px) {
                width: calc(50% - 90px);
            }
            @media (min-width: 1200px) {
                width: calc(50% - 100px);
            }
            .show-mobile {
                display: flex;
                @media (min-width: 992px) {
                    display: none;
                }
            }

            .show-desktop {
                display: none;
                @media (min-width: 992px) {
                    display: flex;
                }
            }
            .icon {
                position: relative;
                margin-right: 10px;
                cursor: pointer;
                &:hover {
                    .tooltip {
                        display: flex;
                    }
                }
                .tooltip {
                    position: absolute;
                    width: max-content;
                    display: none;
                    padding: 5px 10px;
                    justify-content: center;
                    background-color: white;
                    border-radius: 10px;
                    top: 0px;
                    left: 50%;
                    transform: translate(-50%, calc(-100% - 15px));
                    z-index: 2;
                    flex-wrap: nowrap;
                    filter: drop-shadow(0px 0px 10px rgba(74, 74, 74, 0.2));
                    &:after {
                        content: "";
                        clip-path: polygon(50% 60%, 0 0, 100% 0);
                        z-index: 2;
                        position: absolute;
                        width: 15px;
                        height: 15px;
                        transform: translateX(-50%);
                        bottom: -15px;
                        left: 50%;
                        background-color: white;
                    }
                }
                .force-display {
                    display: flex;
                }
            }
            .icon-container {
                display: none;
                position: absolute;
                margin-top: 60px;
                z-index: 2;
                cursor: pointer;
                .icon {
                    margin-bottom: 10px;
                }
            }
        }
    }
    .contents {
        position: relative;
        display: flex;
        justify-content: center;
        .content {
            position: relative;
            width: 90%;
            @media (min-width: 768px) {
                width: 740px;
            }
        }
    }
}
</style>
