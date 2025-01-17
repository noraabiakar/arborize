=====================
NeuronModel Reference
=====================

The :class:`NeuronModel <.core.NeuronModel>` class binds a set of morphologies to a set of
section types. Each section of each morphology can have multiple labels and the mechanisms
associated with each of these labels are inserted into the sections. Synapse types can be
specified on the model and created using :func:`~.core.NeuronModel.create_synapse`. Both
the mechanisms and synapses are specified using `Glia <https://nrn-glia.readthedocs.io>`_.

In lieu of actual documentation, here's an example:

.. code-block:: python

    from arborize import NeuronModel
    from arborize.builders import rotate

    class GolgiCell(NeuronModel):
      morphologies = [('pair-140514-C2-1_split_1.asc', rotate([0., 1., 0.], [1., 0., 0.]))]

      synapse_types = {
          "AMPA_PF": {
              "point_process": 'AMPA',
              "attributes": {
                  "tau_facil": 54, "tau_rec": 35.1, "tau_1": 30, "gmax": 1200, "U": 0.4
              }
          },
          "AMPA_AA": {
              "point_process": 'AMPA',
              "attributes": {
                  "tau_facil": 54, "tau_rec": 35.1, "tau_1": 30, "gmax": 1200, "U": 0.4
              }
          },
          "AMPA_MF": {
              "point_process": ('AMPA', 'granule'),
          },

          "NMDA": {
              "point_process": ('NMDA', 'stellate'),
              "attributes": {
                  "tau_facil": 5, "tau_rec": 8, "tau_1": 1, "gmax": 10000, "U": 0.43
              }
          },
          "GABA": {
              "point_process": 'GABA',
              "attributes": {
              "tau_facil": 0, "tau_rec": 38.7, "tau_1": 1, "gmax": 3200, "U":0.42, "Erev": -65
              }
          }
      }

      section_types = {
          "soma": {
              "mechanisms": ['Leak', 'Nav1_6', 'Kv1_1', 'Kv3_4', 'Kv4_3', 'Kca1_1', 'Kca3_1', 'Ca', 'Cav3_1', ('cdp5', 'CAM_GoC')],
              "attributes": {
                  "Ra": 122, "cm": 1, "ena": 60, "ek": -80, "eca": 137,
                  ("e", "Leak"): -55,
                  ("gmax", "Leak"): 0.00003,
                  ("gbar", "Nav1_6"): 0.14927733727426,
                  ("gbar", "Kv1_1"): 0.00549507510519,
                  ("gkbar", "Kv3_4"): 0.14910988921938,
                  ("gkbar", "Kv4_3"): 0.00406420380423,
                  ("gbar", "Kca1_1"): 0.017643457890359999,
                  ("gkbar", "Kca3_1"): 0.10177335775222,
                  ("gcabar", "Ca"): 0.0087689418803,
                  ("pcabar", "Cav3_1"): 3.407734319e-05,
                  ("TotalPump", "cdp5"): 1e-7,
              }
          },
          "dendrites": {
              "mechanisms": [], "attributes": {}
          },
          "basal_dendrites": {
              "synapses": ['AMPA_AA', 'AMPA_MF', 'NMDA', 'GABA'],
              "mechanisms": ['Leak','Nav1_6','Kca1_1','Kca2_2','Ca',('cdp5', 'CAM_GoC')],
              "attributes": {
                  "Ra": 122, "cm": 2.5, "ena": 60, "ek": -80, "eca": 137,
                  ("e", "Leak"): -55,
                  ("gmax", "Leak"): 0.00003,
                  ("gbar", "Nav1_6"): 0.0080938853145999991,
                  ("gbar", "Kca1_1"): 0.012260527481460001,
                  ("gkbar", "Kca2_2"): 0.016506899583850002,
                  ("gcabar", "Ca"): 0.0013988561771200001,
                  ("TotalPump", "cdp5"): 2e-9,
              }
          },
          "apical_dendrites": {
              "synapses": ['AMPA_PF'],
              "mechanisms": ['Leak', 'Nav1_6', 'Kca1_1', 'Kca2_2', 'Cav2_3', 'Cav3_1', ('cdp5', 'CAM_GoC')],
              "attributes":  {
                  "Ra": 122, "cm": 2.5, "ena": 60, "ek": -80, "eca": 137,
                  ("e", "Leak"): -55,
                  ("gmax", "Leak"): 0.00003,
                  ("gbar", "Nav1_6"): 0.00499506303209,
                  ("gbar", "Kca1_1"): 0.01016375552607,
                  ("gkbar", "Kca2_2"): 0.0024717247914099998,
                  ("gcabar", "Cav2_3"): 0.00128859564935,
                  ("pcabar", "Cav3_1"): 3.690771983e-05,
                  ("TotalPump", "cdp5"): 5e-9,
              }
          },
          "axon": {
              "mechanisms": ['Leak', 'Nav1_6', 'Kv3_4', ('cdp5', 'CAM_GoC')],
              "attributes": {
                  "Ra": 122, "cm": 1, "ena": 60, "ek": -80, "eca": 137,
                  ("e", "Leak"): -55,
                  ("gmax", "Leak"): 0.000001,
                  ("gbar", "Nav1_6"): 0.0115,
                  ("gkbar", "Kv3_4"): 0.0091,
                  ("TotalPump", "cdp5"):  1e-8,
              }
          },
          "axon_initial_segment": {
              "mechanisms": ['Leak', ('HCN1', 'golgi'), 'HCN2', 'Nav1_6', "Ca", 'Kca1_1', 'Km', ('cdp5', 'CAM_GoC')],
              "attributes": {
                  "Ra": 122, "cm": 1, "ena": 60, "ek": -80, "eca": 137,
                  ("e", "Leak"): -55,
                  ("gmax", "Leak"):  0.00003,
                  ("gbar", "Nav1_6"): 0.17233663543618999,
                  ("gbar", "Kca1_1"): 0.10008178886943001,
                  ("gcabar", "Ca"): 0.0059504600114800004,
                  ("gkbar", "Km"): 0.00024381226197999999,
                  ("gbar", "HCN1"): 0.0003371456442,
                  ("gbar", "HCN2"): 0.00030643090764,
                  ("TotalPump", "cdp5"): 1e-8,
              }
          }
      }

      labels = {
          "basal_dendrites": {
              "from": "dendrites",
              "diam": lambda d: d > 1.6
          "apical_dendrites": {
              "from": "dendrites",
              "diam": lambda d: d <= 1.6
          },
          "axon_initial_segment": {
              "from": "axon",
              "id": lambda id: id == 0
          }
      }
