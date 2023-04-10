<template>
  <div>
    <el-tree ref="menuTree" :data="limitdata" show-checkbox node-key="id" accordion :props="defaultProps"
      :check-strictly="true" @check-change="handleMenuCheckChange">
    </el-tree>
  </div>
</template>

<script>
export default {
  data() {
    return {
      limitdata: [
        {
          children: [
            {
              children: [],
              createBy: "admin",
              icon: "menu1-1",
              id: 3,
              lastUpdateBy: "admin",
              lastUpdateTime: "2023/3/31",
              level: "0",
              menuName: "二级菜单1-1",
              parentId: 1,
              parentName: "二级菜单1-1",
              url: "/menu1-1"
            },
            {
              children: [],
              createBy: "admin",
              icon: "menu1-2",
              id: 4,
              lastUpdateBy: "admin",
              lastUpdateTime: "2023/3/31",
              level: "0",
              menuName: "二级菜单1-2",
              parentId: 1,
              parentName: "二级菜单1-2",
              url: "/menu1-2"
            },
            {
              children: [],
              createBy: "admin",
              icon: "menu1-3",
              id: 9,
              lastUpdateBy: "admin",
              lastUpdateTime: "2023/3/31",
              level: "0",
              menuName: "二级菜单1-3",
              parentId: 1,
              parentName: "二级菜单1-3",
              url: "/menu1-3"
            },
          ],
          createBy: "admin",
          icon: "menu1",
          id: 1,
          lastUpdateBy: "admin",
          lastUpdateTime: "2023/3/31",
          level: "0",
          menuName: "一级菜单1",
          parentId: 0,
          parentName: "一级菜单1",
          url: "/menu1"
        },
        {
          children: [
            {
              children: [],
              createBy: "admin",
              icon: "menu2-1",
              id: 5,
              lastUpdateBy: "admin",
              lastUpdateTime: "2023/3/31",
              level: "0",
              menuName: "二级菜单2-1",
              parentId: 2,
              parentName: "二级菜单2-1",
              url: "/menu2-1"
            },
          ],
          createBy: "admin",
          icon: "menu2",
          id: 2,
          lastUpdateBy: "admin",
          lastUpdateTime: "2023/3/31",
          level: "0",
          menuName: "一级菜单2",
          parentId: 2,
          parentName: "一级菜单2",
          url: "/menu2"
        }
      ],
      defaultProps: {
        children: "children",
        label: "menuName",
      },
    };
  },
  methods: {
    // 树节点选择监听
    //节点选中状态发生变化时的回调	共三个参数，  :check-strictly="true"
    //依次为：传递给 data 属性的数组中该节点所对应的对象、节点本身是否被选中、节点的子树中是否有被选中的节点
    handleMenuCheckChange(data, check, subCheck) {
      console.log("勾选", data, check, subCheck);
      // 当前节点被选中时
      if (check) {
        // 子节点选中时同步选中父节点  data.parentId就是打印了勾选这个子节点时对应的对象数据里的parentId
        let parentId = data.parentId;
        this.$refs.menuTree.setChecked(parentId, true, false);
        //  父节点选中时，子节点全选
        if (data.children != null) {
          data.children.forEach((element) => {
            // (key/data, checked, deep) 接收三个参数，
            // 1. 勾选节点的 key 或者 data 2. boolean 类型，
            // 节点是否选中
            // 3. boolean 类型，是否设置子节点 ，默认为 false
            this.$refs.menuTree.setChecked(element.id, true, false);
          });

        }

        // 当前节点不被选中时
      } else {
        // 父节点取消选中时同步取消选中的所有子节点，并且保证有子节点一定会有父节点勾选中
        if (data.children != null) {
          data.children.forEach((element) => {
            this.$refs.menuTree.setChecked(element.id, false, false);
          });
        }
        this.$nextTick(() => {
          let parent = this.$refs.menuTree.getNode(data.parentId)
          // 如果parent存在，即用getNode可以去获得所有的节点的checked情况，如果是父节点该parent不存在
          if (parent) {
            // 注意!!是childNodes,不是children,表示勾选的这个子节点对应父节点的所有子节点信息
            const siblings = parent.childNodes
            // 根据打印值可知道,当全部子节点不勾选时,someChecked会是false
            const allChecked = siblings.every(sibling => sibling.checked)
            const someChecked = siblings.some(sibling => sibling.checked)
            console.log("someChecked", someChecked)
            // 子节点全部不勾选的时候,父节点也不选中
            if (!someChecked) {
              let parentId = data.parentId;
              this.$refs.menuTree.setChecked(parentId, false, false);
            }
          }
        })
      }
    },
  }
};
</script>
