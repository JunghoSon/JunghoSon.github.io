---
layout: post
title: "모달 type 메모"
date: 2025-06-04
---

## store

```typescript
import { ComponentProps } from "react";

import { create } from "zustand";
import { devtools } from "zustand/middleware";
import { immer } from "zustand/middleware/immer";

import { COMMON_MODAL_IDS } from "@/component/modals/constants";
import { MODAL_COMPONENTS } from "@/component/modals/modalComponents";
import { postMessageDimOff, postMessageDimOn } from "@/utils/iframeUtils";

export type ModalComponentsType = typeof MODAL_COMPONENTS;
export type ModalKeyType = keyof ModalComponentsType;
export type ModalEntry<K extends ModalKeyType = ModalKeyType> = {
  id: K;
  props?: ComponentProps<ModalComponentsType[K]>;
  isCloseTriggered?: boolean;
};

export type CommonModalStore = {
  modals: ModalEntry[];
};

export type CommonModalActions = {
  actions: {
    openModal: <K extends ModalKeyType>(
      modalId: K,
      props?: ComponentProps<ModalComponentsType[K]>
    ) => void;
    closeModal: (modalId: ModalKeyType) => void;
    closeModalWithoutDimHiding: (modalId: ModalKeyType) => void;
    closeAllModals: () => void;
    isLastModal: (modalId: ModalKeyType) => boolean;
    reset: () => void;
  };
};

const initialState: CommonModalStore = {
  modals: [],
};

export const useCommonModalStore = create<
  CommonModalStore & CommonModalActions
>()(
  devtools(
    immer((set, get) => ({
      ...initialState,
      actions: {
        openModal: (modalId, props): void => {
          set((state) => {
            if (state.modals.some((m) => m.id === modalId)) return;
            state.modals.push({ id: modalId, props });

            const modalCount = document.querySelectorAll(".modal-area")?.length;
            if (!modalCount) postMessageDimOn();
          });
        },
        closeModal: (modalId) => {
          set((state) => {
            const targetModal = state.modals.find(
              (modal) => modal.id === modalId
            );
            if (targetModal) targetModal.isCloseTriggered = true;

            const modalCount = document.querySelectorAll(".modal-area")?.length;
            if (modalCount === 1) postMessageDimOff();
            setTimeout(() => {
              set((currentStore) => {
                return {
                  modals: currentStore.modals.filter(
                    (modal) => modal.id !== modalId
                  ),
                };
              });
              const count = document.querySelectorAll(".modal-area")?.length;
              if (count === 1) postMessageDimOff();
            }, 420);
          });
        },
        closeModalWithoutDimHiding: (modalId) => {
          set((state) => {
            state.modals = state.modals.filter((modal) => modal.id !== modalId);
          });
        },
        closeAllModals: () => {
          set((state) => {
            state.modals = [];
          });
        },
        isLastModal: (modalId) => {
          const modalIds = get()
            .modals.filter((m) => !COMMON_MODAL_IDS.includes(m.id))
            .map((m) => m.id);

          const modalIndex = modalIds.indexOf(modalId);
          return modalIndex === modalIds.length - 1;
        },
        reset: () => {
          set(initialState);
        },
      },
    }))
  )
);

export const useCommonModalActions = () =>
  useCommonModalStore((s) => s.actions);
export const useCommonModalCloseTriggered = (modalId: ModalKeyType) =>
  useCommonModalStore(
    (s) => s.modals.find((m) => m.id === modalId)?.isCloseTriggered
  );
```

## TYPE

```typescript

```

##

```typescript
import ActivityBadgeDetailModal from "@/component/modals/careerplan/ActivityBadgeDetailModal";
import ActivityCertificateDetailModal from "@/component/modals/careerplan/ActivityCertificateDetailModal";
import ActivityLearningDetailModal from "@/component/modals/careerplan/ActivityLearningDetailModal";
import DesiredJobWarningModal from "@/component/modals/careerplan/DesiredJobWarningModal";
import DialogModal from "@/component/modals/DialogModal";
import CompleteResumeUploadModal from "@/component/modals/front/CompleteResumeUploadModal";
import LoadingModal from "@/component/modals/front/LoadingModal";
import ResumeRegistrationModal from "@/component/modals/front/resume/ResumeRegistrationModal/ResumeRegistrationModal";
import ReUploadResumeModal from "@/component/modals/front/resume/ReUploadResumeModal";
import UserSkillReportModal from "@/component/modals/front/UserSkillReportModal";
import JobDetailCompanyModal from "@/component/modals/job/JobDetailCompanyModal";
import JobDetailMarketModal from "@/component/modals/job/JobDetailMarketModal";
import JobInfoModal from "@/component/modals/job/JobInfoModal";
import JobSkillDetailModal from "@/component/modals/job/JobSkillDetailModal";
import MtiJobDetailModal from "@/component/modals/mti/MtiJobDetailModal";
import MtiMarketJobDetailModal from "@/component/modals/mti/MtiMarketJobDetailModal";
import MtiSkillDetailModal from "@/component/modals/mti/MtiSkillDetailModal";
import MtiSkillProfileStatusModal from "@/component/modals/mti/MtiSkillProfileStatusModal";
import MtiUserProfileModal from "@/component/modals/mti/MtiUserProfileModal";
import AIDraftInfoModal from "@/component/modals/one-on-one/AIDraftInfoModal";
import FeedbackCompleteModal from "@/component/modals/one-on-one/FeedbackCompleteModal";
import ProgressLoadingModal from "@/component/modals/one-on-one/ProgressLoadingModal";
import ReviewSelectModal from "@/component/modals/one-on-one/ReviewSelectModal";
import SearchMemberModal from "@/component/modals/one-on-one/SearchMemberModal";
import UserRequestCompleteModal from "@/component/modals/one-on-one/UserRequestCompleteModal";
import AddCertificationModal from "@/component/modals/profile/AddCertificationModal";
import AddEducationModal from "@/component/modals/profile/AddEducationModal";
import AddProjectModal from "@/component/modals/profile/AddProjectModal";
import CareerInfoModal from "@/component/modals/profile/CareerInfoModal";
import CompareSkillWithCurrentJobModal from "@/component/modals/profile/CompareSkillWithCurrentJobModal";
import LearningModal from "@/component/modals/profile/LearningModal";
import RepresentativeMarketJobModal from "@/component/modals/profile/RepresentativeMarketJobModal/RepresentativeMarketJobModal";
import ResumeDownloadModal from "@/component/modals/profile/resume/ResumeDownloadModal";
import SearchLearningModal from "@/component/modals/profile/SearchLearningModal";
import SkillHistoryModal from "@/component/modals/profile/SkillHistoryModal";
import SkillProfileProgressModal from "@/component/modals/profile/SkillProfileProgressModal";
import GrowthLevelUpSkillModal from "@/component/modals/progress/GrowthLevelUpSkillModal";
import GrowthNewSkillModal from "@/component/modals/progress/GrowthNewSkillModal";
import CompleteSkillSettingModal from "@/component/modals/skill/CompleteSkillSettingModal";
import KeywordModal from "@/component/modals/skill/KeywordModal";
import SearchJobsModal from "@/component/modals/skill/SearchJobsModal";
import SearchSkillModal from "@/component/modals/skill/SearchSkillModal";
import SkillLevelModal from "@/component/modals/skill/skill-level/SkillLevelModal";
import TeamSkillLevelModal from "@/component/modals/skill/skill-level/TeamSkillLevelModal";
import SkillDetailModal from "@/component/modals/skill/SkillDetailModal";
import SkillLevelGuideModal from "@/component/modals/skill/SkillLevelGuideModal";
import TopSkillModal from "@/component/modals/skill/TopSkillModal";
import GoalMemberListModal from "@/component/modals/team/GoalMemberListModal";
import GoalSettingModal from "@/component/modals/team/GoalSettingModal";
import MemberListModal from "@/component/modals/team/MemberListModal";
import MemberSettingModal from "@/component/modals/team/MemberSettingModal";
import TermsModal from "@/component/modals/terms/TermsModal";
import ViewUserProfileModal from "@/component/modals/view/ViewUserProfileModal";
import ViewUserProgressModal from "@/component/modals/view/ViewUserProgressModal";
import WarningModal from "@/component/modals/warning/WarningModal";

export const MODAL_COMPONENTS = {
  ADD_EDUCATION: AddEducationModal,
  SEARCH_SKILL: SearchSkillModal,
  SKILL_LEVEL: SkillLevelModal,
  SKILL_LEVEL_GUIDE: SkillLevelGuideModal,
  ADD_PROJECT: AddProjectModal,
  SKILL_DETAIL: SkillDetailModal,
  ADD_CERTIFICATION: AddCertificationModal,
  SEARCH_LEARNING: SearchLearningModal,
  LEARNING: LearningModal,
  CAREER_INFO: CareerInfoModal,
  TOP_SKILL: TopSkillModal,
  KEYWORD: KeywordModal,
  PROFILE_PROGRESS: SkillProfileProgressModal,
  JOB_DETAIL_COMPANY: JobDetailCompanyModal,
  JOB_DETAIL_MARKET: JobDetailMarketModal,
  JOB_SKILL_DETAIL: JobSkillDetailModal,
  JOB_INFO: JobInfoModal,
  SEARCH_JOBS: SearchJobsModal,
  DESIRED_JOB_WARNING: DesiredJobWarningModal,
  ACTIVITY_LEARNING_DETAIL: ActivityLearningDetailModal,
  ACTIVITY_BADGE_DETAIL: ActivityBadgeDetailModal,
  ACTIVITY_CERTIFICATE_DETAIL: ActivityCertificateDetailModal,
  SKILL_HISTORY: SkillHistoryModal,
  REPRESENTATIVE_MARKET_JOB: RepresentativeMarketJobModal,
  COMPARE_SKILL_WITH_CURRENT_JOB: CompareSkillWithCurrentJobModal,
  RESUME_DOWNLOAD: ResumeDownloadModal,
  //
  // SETUP
  TERMS: TermsModal,
  COMPLETE_RESUME_UPLOAD: CompleteResumeUploadModal,
  USER_SKILL_REPORT: UserSkillReportModal,
  RE_UPLOAD_RESUME: ReUploadResumeModal,
  COMPLETE_SKILL_SETTING: CompleteSkillSettingModal,
  RESUME_REGISTRATION: ResumeRegistrationModal,
  //
  // GROWTH_NEW_SKILL
  GROWTH_NEW_SKILL: GrowthNewSkillModal,
  GROWTH_LEVEL_UP_SKILL: GrowthLevelUpSkillModal,
  //
  // TEAM
  GOAL_SETTING: GoalSettingModal,
  MEMBER_SETTING: MemberSettingModal,
  MEMBER_LIST: MemberListModal,
  GOAL_MEMBER_LIST: GoalMemberListModal,
  TEAM_SKILL_LEVEL: TeamSkillLevelModal,
  //
  // 1 on 1
  PROGRESS_LOADING: ProgressLoadingModal,
  FEEDBACK_COMPLETE: FeedbackCompleteModal,
  SEARCH_MEMBER: SearchMemberModal,
  REVIEW_SELECT: ReviewSelectModal,
  USER_REQUEST_COMPLETE: UserRequestCompleteModal,
  AI_DRAFT_INFO: AIDraftInfoModal,
  //
  // MTI
  MTI_USER_PROFILE: MtiUserProfileModal,
  MTI_JOB_DETAIL: MtiJobDetailModal,
  MTI_MARKET_JOB_DETAIL: MtiMarketJobDetailModal,
  MTI_SKILL_DETAIL: MtiSkillDetailModal,
  MTI_SKILL_PROFILE_STATUS: MtiSkillProfileStatusModal,
  // VIEW
  VIEW_USER_PROFILE: ViewUserProfileModal,
  VIEW_USER_PROGRESS: ViewUserProgressModal,
  //
  // COMMON
  LOADING: LoadingModal,
  DIALOG: DialogModal,
  WARNING: WarningModal,
};
```
