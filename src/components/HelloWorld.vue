<template>
<div class="full">
    <ejs-diagram
        id="diagram"
        :width="width"
        :height="height"
        :nodes="nodes"
        :connectors="connectors"
        :getNodeDefaults="getNodeDefaults"
        :getConnectorDefaults="getConnectorDefaults"
        v-on:click="clicked($event)"
    ></ejs-diagram>
</div>
</template>
<script>
import {
    DiagramPlugin,
    PortVisibility,
    AnnotationConstraints,
    contains
} from "@syncfusion/ej2-vue-diagrams";
import icon from "../assets/group.svg";

let constraint = [
    {
        constraints: AnnotationConstraints.ReadOnly
    }
];

export default {
    data() {
        return {
            width: "100%",
            height: "90%",
            nodes: [],
            connectors: [],
            getNodeDefaults: node => {
                node.height = 100;
                node.width = 100;
                node.annotations = constraint;
                return node;
            },
            getConnectorDefaults: con => {
                con.annotations = constraint;
                con.type = "Orthogonal";
                return con;
            },
            clickComp: null,
            downTimer: null
        };
    },
    methods: {
        clicked(e) {
            if (e.actualObject !== undefined) {
                if (this.clickComp == e.actualObject.properties.id) {
                    alert(
                        "nama : "+e.actualObject.properties.id +"\ntype : " +e.actualObject.propName
                    );
                    clearTimeout(this.downTimer);
                    this.clickComp = null
                } else {
                    clearTimeout(this.downTimer);
                    this.clickComp = e.actualObject.properties.id
                    var that = this
                    this.downTimer = setTimeout(function() {
                        that.clickComp = null
                    }, 500);
                }
            }
        },
        named(name) {
            return `<div style="background:white;height:100%;width:100%;border:2px solid #eaedf3;border-radius:10px;display: flex;align-items:center;justify-content:center;flex-flow:column wrap">\
            <div style="background-image:url(${icon});background-repeat:no-repeat;height:44px;width:44px"></div>\
            <p style="font-size:11pt;margin-top:0px">${name}</p>\
            </div>`;
        }
    },
    created() {
        let nodeName = [
            "trigger",
            "masuk atas",
            "masuk bawah",
            "keluarkan"
        ];
        this.nodes = [
            {
                id: nodeName[0],
                offsetX: 100,
                offsetY: 200,
                shape: {
                    type: "HTML",
                    content: this.named(nodeName[0])
                }
            },
            {
                id: nodeName[1],
                offsetX: 300,
                offsetY: 100,
                shape: {
                    type: "HTML",
                    content: this.named(nodeName[1])
                }
            },
            {
                id: nodeName[2],
                offsetX: 300,
                offsetY: 300,
                shape: {
                    type: "HTML",
                    content: this.named(nodeName[2])
                }
            },
            {
                id: nodeName[3],
                offsetX: 500,
                offsetY: 200,
                shape: {
                    type: "HTML",
                    content: this.named(nodeName[3])
                }
            }
        ];

        this.connectors = [
            {
                id: "connector1",
                sourceID: nodeName[0],
                targetID: nodeName[1]
            },
            {
                id: "connector2",
                sourceID: nodeName[0],
                targetID: nodeName[2]
            },
            {
                id: "connector3",
                sourceID: nodeName[1],
                targetID: nodeName[3]
            },
            {
                id: "connector4",
                sourceID: nodeName[2],
                targetID: nodeName[3]
            }
        ];
    }
};
</script>
<style>
@import "../../node_modules/@syncfusion/ej2-vue-diagrams/styles/material.css";
.full{
 height: 90vh;
 width: 100%;   
}
</style>
