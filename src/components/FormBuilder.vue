<template>
    <div :class="[styles.CONTAINER.FLUID, 'form-padding', 'vue-form-builder']">

        <!-- top configuration -->
        <div class="form-configuration-block pbottom-10">
            <div class="row">
                <div class="col-md-8">
                    <div class="btn-row">
                    <FormConfiguration
                    :permissions="permissions"
                    v-model="formData.formConfig"
                    />
                    </div>
                </div>

                <div class="col-md-4">
                <AddSectionControl
                v-if="permissions.canAddSection"
                @addSectionNotify="addSection"
                />
                </div>
            </div>

            <!-- form headline -->
            <div class="form-headline-container" v-show="formData.formConfig.isShowHeadline">
                <h1 v-text="formData.formConfig.headline"></h1>
                <p v-text="formData.formConfig.subHeadline"></p>
            </div>

                <SectionContainer
                v-for="(sectionData) in sortedSections"
                :section="sectionData"
                :rows="formData.rows"
                :controls="formData.controls"
                :key="sectionData.uniqueId"
                :permissions="permissions"
                />
          
        </div>
       
        <!-- global stuff -->
        <GlobalSidebar
            :formData="formData"
            :permissions="permissions"
        />
        <GlobalModal
            :formData="formData"
            :permissions="permissions"
        />

        <hr>

        <p class="copyright-text" v-text="copyrightText"></p>
    </div>
</template>

<script>
    import AddSectionControl from "@/views/builder/add-controls/AddSectionControl";
    import {MAIN_CONSTANTS} from "@/configs";
    import SectionContainer from "@/views/builder/SectionContainer";
    import FormBuilderBusiness from "@/mixins/form-builder-mixins";
    import FormConfiguration from "@/views/builder/FormConfiguration";
    import GlobalSidebar from "@/views/builder/GlobalSidebar";
    import GlobalModal from "@/views/builder/GlobalModal";
    import DefaultPermission from "@/configs/roles";

    export default {
        name: "FormBuilder",
        components: {
            GlobalModal,
            GlobalSidebar,
            FormConfiguration,
            SectionContainer,
            AddSectionControl
        },
        mixins: FormBuilderBusiness,

        props: {
            permissions: {
                type: Object,
                default: () => {
                    return DefaultPermission
                }
            }
        },

        data: () => ({
            formData: {
                formConfig: {},
                sections: {},
                rows: {},
                controls: {},
            },
        }),

        created() {
            if (this.value && typeof this.value === 'object') {
                this.mapping(this.value)
            } else {
                this.createDefaultData()
            }
        },

        computed: {
            /**
             * Copyright Text
             * @returns {string}
             */
            copyrightText() {
                return MAIN_CONSTANTS.COPYRIGHT
            }
        }
    }
</script>
