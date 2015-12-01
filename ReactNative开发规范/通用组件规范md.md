# 通用组件规范
##目录结构，参考【目录结构】
##代码结构 由上到下顺序
###_compName
组件名称，必填，日志信息中会打印出的名字信息。
###options
组件选项，必填，定义此组件的属性信息。可以方便的知道组件都有哪些属性。可以利用checkProps进行组件属性校验工作。

	options:{
	        getUrl:{
	            must:true,
	            type:'function'
	        },
	        getData:{
	            must:true,
	            type:'function',
	        },
	        renderRow:{
	            must:true,
	            type:'function',
	        },
	    },     	    	    
###属性列表
private属性以_开头。

public属性没有命名要求。

无论公有属性还是私有属性一般对外是只读的。要想暴露操作功能，请提供public方法。

###public方法列表
public方法没有命名要求。

###getInitialState
状态声明。具体参考react官网。

###组件生命周期事件
例如 componentWillMount

要求必须要在componentWillMount中，用属性检查器执行检查。

	componentWillMount(){
        checkProps.check(this);
    },

###render
render函数。具体参考react官网。

###private方法列表
私有方法以_开头。

###示例代码
```


'use strict'


var React=require('react-native');
var css=require('./ListViewBindUrl.css');
var Loading=require('../Loading/Loading');
var checkProps=require('../CheckProps/CheckProps');


var {
    AppRegistry,
    Component,
    StyleSheet,
    Text,
    View,
    Image,
    Navigator,
    TouchableHighlight,
    TouchableOpacity,
    Platform,
    StatusBarIOS,
    ScrollView,
    TextInput,
    Dimensions,
    Animated,
    LayoutAnimation,
    InteractionManager,
    ListView,
    }=React;


var ListViewBindUrl =React.createClass({


    //组件名称，一般输出调试信息的时候会比较有用
    _compName:'ListViewBindUrl',

    //组件的选项，定义在组件上，通过this.props来使用，
    //通过查看此选项，可以方便的知道组件都有哪些属性
    //利用checkProps可以方便的进行校验工作
    options:{
        getUrl:{
            must:true,
            type:'function'
        },
        getData:{
            must:true,
            type:'function',
        },
        renderRow:{
            must:true,
            type:'function',
        },
    },

    curPage:0,
    data:[],

    getInitialState(){
        return {
            dataSource:new ListView.DataSource({
                rowHasChanged: (r1, r2) => true
            }),

            _loading:false,
            _noData:false,
        };
    },


    componentWillMount(){

        checkProps.check(this);

        this._queryData();

    },


    render(){

        var props=this.props;

        return (

            <ListView dataSource={this.state.dataSource} keyboardShouldPersistTaps={true}
                      renderRow={this.props.renderRow}
                      onEndReached={this._endReached }
                      onEndReachedThreshold={200}
                      renderFooter={this._renderFooter}
                />

        );
    },


    _renderFooter(){

        if(this.state._noData==true){
            return(
                <View style={css.footerLoadingView}>
                    <Text> 没有数据了.. </Text>
                </View>
            );
        }

        if(this.state._loading==true){
            return(
                <Loading style={css.footerLoadingView} show={true} />
            );
        }

    },


    _endReached(){
        this._queryData();
    },

    _queryData(){

        if(this.state._loading==true){
            console.log('尚有查询请求未完成，不能执行新的请求.');
            return;
        }

        if(this.state._noData==true){
            console.log('服务器已经没有数据了，不能执行新的请求.');
            return;
        }

        this.setState({_loading:true});

        var props=this.props;
        var getUrl=props.getUrl;

        var url=getUrl(this.curPage);
        if(!url){
            console.error('返回的url为空。');
            return;
        }


        console.log('开始发起新的请求....');

        fetch(url)
            .then((res)=>res.json())
            .then((res)=>{

                console.log('请求成功.');

                var getData=props.getData;
                var data=getData(res);

                if(!data||data.length==0){
                    this.setState({_noData:true});
                    console.log('服务器已经没有数据了.');
                }else{
                    this.data=this.data.concat(data);
                    var ds=this.state.dataSource.cloneWithRows(this.data);
                    this.curPage=this.curPage+1;
                    this.setState({dataSource:ds});
                }

                console.log('更新view成功.');

            })
            .catch((error)=>console.error(error))
            .done(()=>{
                this.setState({_loading:false});
            });


    },



});

module.exports=ListViewBindUrl;


```






