/**
 * @class youtubeAPI
 *  This script will pull all of the lojistic videos and add them to the page dynamically
 *  @author @jppope | jonpaul@lojistic.com
 *  @dependencies fancybox
 *
 *  note: we do the API pull on the server to avoid issues
 *  #globals
 *  variables / contstants / references
 */
var url = '/videos/index';
var playlistURL ='/videos/company';
/**
 * @method init
 * This function initializes all of the scripts and keeps functions out of the global scope
 */
function initVideos() {
    $(document).ready(function () {
        fancybox();
        apiCall();
        odometerInstance();
    });
}

/**
 * @method initCareerVideos
 * This function initializes all of the scripts and keeps functions out of the global scope
 */
function initCareerVideos() {
    $(document).ready(function () {
        fancybox();
        careerApiCall();
    });
}

/**
 * @method fancybox
 * fancybox lightbox ... keeping it out of the global scope
 */
function fancybox() {
    $('.fancybox').fancybox({
        loop: false,
        afterLoad: function () {
            $('#youtube_player')[0].contentWindow.postMessage(
                '{"event":"command","func":"' + 'stopVideo' + '","args":""}', '*'
            );
        }
    });
}

/**
 * @method apiCall
 * function wrapper for getting call that gets json
 */
function apiCall() {
    $.getJSON(url, function (data) {
        //note: path to videos data.items[i].snippet.resourceId.videoId;
        var videoItems = data.items;
        var videoAndDesc = [];
        _.each(videoItems, function(item){
            var vid =  {};
            vid.video = "https://www.youtube.com/embed/";
            vid.video += item.snippet.resourceId.videoId;
            vid.video += "?enablejsapi=1&version=3&playerapiid=ytplayer&rel=0&showinfo=0";
            vid.text = item.snippet.title;
            vid.img = item.snippet.thumbnails.medium.url;
            videoAndDesc.push(vid);
        });

        var fourVideos = _.sample(videoAndDesc,5);
        vueVideos(fourVideos);
        videoGallery(videoAndDesc);
    });
}

/**
 * @method careerApiCall
 * function wrapper for getting call that gets json
 */
function careerApiCall() {
    $.getJSON(playlistURL, function (data) {
        //note: path to videos data.items[i].snippet.resourceId.videoId;
        var videoItems = data.items;
        var videoAndDesc = [];
        _.each(videoItems, function(item){
            var vid =  {};
            vid.video = "https://www.youtube.com/embed/"
            vid.video += item.snippet.resourceId.videoId;
            vid.video += "?enablejsapi=1&version=3&playerapiid=ytplayer&rel=0&showinfo=0";
            vid.text = item.snippet.title;
            vid.img = item.snippet.thumbnails.medium.url;
            videoAndDesc.push(vid);
        });

        var threeVideos = _.sample(videoAndDesc,4);
        careerVideoSection(threeVideos);
    });
}

/**
 * @method vueSidebarVideos
 * this method constructs the initial main video on the lojistic.com/videos page using vuejs
 * @params arr
 */
function vueVideos(arr){
    var main = arr[0];
    arr.shift();
    var three = arr;
    var videoSample = new Vue({
        el: '#logistic-video',
        data: {
            main: {},
            first: main,
            videos: three,
            images: three
        },
        methods: {
            setMainVideo: function ($index){
                this.$data.first = this.$data.videos[$index];
                }
            }
        }
    )
}

/**
 * @method vueSidebarVideos
 * this method constructs the initial main video on the lojistic.com/videos page using vuejs
 * @params arr
 */
function careerVideoSection(arr){
    var main = arr[0];
    arr.shift();
    var three = arr;
    var videoSample = new Vue({
            el: '#careers-video',
            data: {
                main: {},
                first: main,
                videos: three,
                images: three
            },
            methods: {
                setMainVideo: function ($index){
                    this.$data.first = this.$data.videos[$index];
                }
            }
        }
    )
}

/**
 * @method videoGallery
 * this method constructs the gallery of videos using vuejs
 * @params vidsArr
 */
function videoGallery(vidsArr){
    var videoSample = new Vue({
            el: '#videoGallery',
            data: {
                main: {},
                items: vidsArr,
                images: vidsArr
            }
        }
    )
}

/**
 *  @method odometerInstance
 *  this method initiates an instance of odometer js
 */
function odometerInstance() {
    var elt = document.getElementById("roller");
    var texts = ["#whereInTheWorldIsParcel","#LateShipmentsSuck"];
    elt.textroller  = new TextRoller({
        el: elt,
        values: texts,    // an array of texts.     default : [el.innerHtml]
        align: "left",    // right, left or middle. default : middle
        delay: 1500,      // in milliseconds,       default : 5000
        loop: false       // at the end, restart.   default : true
    });
}


// https://developers.google.com/youtube/iframe_api_reference










